<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let genderData, index, width, height;

    let country = "United States";
    let filteredData = genderData.filter((d) => d.entity === country);
    let countryInput = ""; // This will bind to the input field
    let countrySuggestions = []; // This will store filtered suggestions

    // Extract unique country names for suggestions
    let uniqueCountries = Array.from(new Set(genderData.map(d => d.entity)));

    // Update suggestions based on input
    $: {
        if (countryInput) {
            countrySuggestions = uniqueCountries.filter(c => 
                c.toLowerCase().startsWith(countryInput.toLowerCase())
            );
        } else {
            countrySuggestions = [];
        }
    }

    function search() {
        // Update the country variable with the current input
        country = countryInput;
        let lowerCaseCountry = country.toLowerCase();
        filteredData = genderData.filter((d) => d.entity.toLowerCase() === lowerCaseCountry);
        countryInput = null;
    }

    function selectCountry(selectedCountry) {
        countryInput = selectedCountry;
        search(); // Trigger the search immediately upon selection
    }

    const margin = {top: 90, right: 710, bottom: 180, left: 100};
    width = 1300;
    height = 680;

    let xScale, yScale, xAxis, yAxis;

    $: {xScale = d3.scaleLinear()
          .domain([0, Math.max(10, d3.max(filteredData, d => d.data[0].female))])
          .range([margin.left, width - margin.right]);
          
    yScale = d3.scaleLinear()
          .domain([0, Math.max(8, d3.max(filteredData, d => d.data[0].male))])
          .range([height - margin.bottom, margin.top]);

    d3.select(xAxis)
        .call(d3.axisBottom(xScale))
        .attr('stroke-width', 2)
        .attr('font-family', 'Verdana')
        .selectAll('.tick line')
        .attr('font-size', 12);
      
    d3.select(yAxis)
        .call(d3.axisLeft(yScale))
        .attr('stroke-width', 2)
        .attr('font-family', 'Verdana')
        .selectAll('.tick line')
        .attr('font-size', 12);
    }
    
    //     for (let i = 0; i < filteredData.length; i++) {
    //     console.log(filteredData[i].data[0].female);
    // }

    let highlightedCircle = null;

    function handleHover(i) {
        highlightedCircle = i;
    }

    function handleLeave() {
        highlightedCircle = null;
    } 

    let tooltipPt = null;

    function onPointerMove(event, i) {
        tooltipPt = filteredData[i];
    }

    function onPointerLeave() {
        tooltipPt = null;
    }

</script>

<main style="display: flex; margin-top: 5%;">

    <svg {width}{height}>
        
        <g class='points'>
            {#each filteredData as d, i}
              <circle
                key={i}
                cx={xScale(d.data[0].female)}
                cy={yScale(d.data[0].male)}
                fill='#e3c466'
                stroke='#3b3729'
                stroke-width='1.5' 
                r="5"
                class:highlighted={highlightedCircle === i}
                on:mouseover={(event) => 
                    {handleHover(i);
                    onPointerMove(event, i);}}
                on:mouseout={(event) => 
                                {handleLeave();
                                onPointerLeave();}}
              />
            {/each}
        </g>

        <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
        <g transform="translate({margin.left}, 0)"
        bind:this={yAxis} />

        <g class="axis-title">
            <text transform={`translate(${350}, ${height - margin.bottom + 50})`} text-anchor="middle" font-family="Verdana">Share of Female Population With Anxiety Disorders (%)</text>
            <text transform={`translate(${350}, ${height - margin.bottom + 90})`} text-anchor="middle" font-family="Verdana" font-size="13px">*Each point represents value in a specific year</text>
        </g>

        <g class="axis-title">
            <text transform={`translate(${margin.left - 50}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Share of Male Population With Anxiety Disorders (%)</text>    
        </g>

        <!-- Title -->
        <text x="55%" y="45" font-family="Verdana" font-size="18" text-anchor="middle" class="title">Share of Population with Anxiety in {country}</text>

        {#if tooltipPt}
            <g transform={`translate(${xScale(tooltipPt.data[0].female)}, ${yScale(tooltipPt.data[0].male)})`}>
                <rect x="-120" y="-30" width="120" height="50" fill="white" stroke="black" />
                <text x="-115" y="-20" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Year: {tooltipPt.year}</text>
                <text x="-115" y="-5" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Female: {tooltipPt.data[0].female}</text>
                <text x="-115" y="10" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Male: {tooltipPt.data[0].male}</text>
            </g>
        {/if}
    </svg>

    <div style="margin-left: 50px; margin-right: 100px; width: 100%;">
        <h1 style="font-family: Futura; font-size: 34px; margin-bottom: 20px; line-height: 1.6;">The Silent Struggle: Anxiety Among Males</h1>
        <div style="font-family: Verdana; font-size: 18px; text-align: justify; line-height: 1.6;">
            <p>Data suggests that a higher share of the female population has an anxiety disorder compared to males. However, data also shows that the share of the male population with anxiety is not significantly less than that of females. In the United States during the past decade, the percentage of males with anxiety has been around two-thirds of that of females, representing tens of millions of individuals.</p>
            <p>Some men may avoid discussing the anxiety they experience if they don't perceive it as serious enough for intervention. Approximately 40% of men have never spoken to anyone about their mental health. A likely reason for this phenomenon is the gender stereotype that still prevails in our society.</p>
        </div>

        <!-- <div style="display: flex; justify-content: center; margin-top: 20px;">
            <input bind:value={country} type="text" class="form-control" style="margin-right: 5px;"/>
            <button on:click={search} class="btn btn-light">Search</button>
        </div> -->

        <div style="display: flex; justify-content: center; margin-top: 20px;">
            <input bind:value={countryInput} style="" type="text" class="form-control" placeholder="Search for a country..."/>
            {#if countrySuggestions.length}
                <ul class="suggestions-list" style="margin-top: 42px; margin-left: -30%">
                    {#each countrySuggestions as suggestion}
                        <li on:click={() => selectCountry(suggestion)}>{suggestion}</li>
                    {/each}
                </ul>
            {/if}
            <button on:click={search} class="btn btn-light" style="margin-left: 9px;">Search</button>
        </div>
    </div>

</main>


<style>
    .suggestions-list {
        list-style-type: none;
        margin: 0;
        padding: 0;
        position: absolute;
        background-color: #fff;
        border: 1px solid #ddd;
        z-index: 1000;
    }

    .suggestions-list li {
        padding: 8px 12px;
        cursor: pointer;
    }

    .suggestions-list li:hover {
        background-color: #f0f0f0;
    }

    .highlighted {
        fill: #fce6a4;
    }
</style>
