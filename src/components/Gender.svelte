<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let genderData, width, height;

    let country = "United States";
    let filteredData = genderData.filter((d) => d.entity === country);
    // console.log(filteredData);
    

    function search() {
        filteredData = genderData.filter((d) => d.entity === country)[0].data;
        console.log(filteredData);
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
</script>

<main>
    <h1>Anxiety Prevalence By Gender</h1>
    <svg {width}{height}>
        <g class='points'>
            {#each filteredData as d, i}
              <circle
                key={i}
                cx={xScale(d.data[0].female)}
                cy={yScale(d.data[0].male)}
                fill='black'
                r="5"
              />
              <!-- <text x='500' y='500'>{d.data[0].female}</text>
              <text x='500' y='525'>{d.data[0].male}</text> -->
            {/each}
        </g>

        <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
        <g transform="translate({margin.left}, 0)"
        bind:this={yAxis} />
    </svg>
</main>