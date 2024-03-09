<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let genderData, index, width, height;

    let country = "United States";
    let filteredData = genderData.filter((d) => d.entity === country);
    // console.log(filteredData);
    

    function search() {
        filteredData = genderData.filter((d) => d.entity === country);
    }

    const margin = {top: 90, right: 700, bottom: 180, left: 80};
    width = 1300;
    height = 680;

    let xScale;
    let yScale;
    let xAxis;
    let yAxis;

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

<main>
    <h1 >Anxiety Prevalence By Gender</h1>

    <div style="display: flex; justify-content: center;">
        <input bind:value={country} type="text" />
        <button on:click={search}>Search</button>
    </div>
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
        </g>

        <g class="axis-title">
            <text transform={`translate(${margin.left - 50}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Share of Male Population With Anxiety Disorders (%)</text>    
        </g>

        {#if tooltipPt}
            <g transform={`translate(${xScale(tooltipPt.data[0].female)}, ${yScale(tooltipPt.data[0].male)})`}>
                <rect x="5" y="-30" width="120" height="50" fill="white" stroke="black" />
                <text x="10" y="-20" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Year: {tooltipPt.year}</text>
                <text x="10" y="-5" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Female: {tooltipPt.data[0].female}</text>
                <text x="10" y="10" font-size="12" font-weight="regular" font-family="Arial, sans-serif" text-anchor="start" dominant-baseline="central">Male: {tooltipPt.data[0].male}</text>
            </g>
        {/if}
    </svg>
</main>

<style>
    .highlighted {
        fill: #fce6a4;
    }
</style>
