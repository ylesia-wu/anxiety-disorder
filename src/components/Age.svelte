<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let ageData, index, width, height;
    
    let country = "United States";
    let year = 2003;
    let filteredData = ageData.filter((d) => (d.entity === country) & (d.year === year))[0].data;

    function search() {
        filteredData = ageData.filter((d) => (d.entity === country) & (d.year === year))[0].data;
        console.log(filteredData);
    }
    // for (let i = 0; i < ageData.length; i++) {
    //     console.log(ageData[i].entity);
    // }
    
    

    const data = [
      {age: "5-14", value: 2.266049},
      {age: "15-19", value: 5.411234},
      {age: "20-24", value: 7.232454},
      {age: "25_29", value: 8.22605},
      {age: "30-34", value: 8.645155},
      {age: "35-39", value: 8.778514},
      {age: "40-44", value: 8.709814},
      {age: "45-49", value: 8.355194},
      {age: "50-54", value: 7.798448},
      {age: "55-59", value: 7.150492},
      {age: "60-64", value: 6.486229},
      {age: "65-69", value: 5.975492},
    ];

    const margin = {top: 90, right: 40, bottom: 220, left: 750};
    width = 1300;
    height = 680;

    let xScale;
    let yScale;
    let xAxis;
    let yAxis;

    // $: {yScale = d3.scaleBand()
    //           .domain(data.map(d => d.age))
    //           .range([margin.top, height - margin.bottom])
    //           .padding(0.1);

    //   xScale = d3.scaleLinear()
    //       .domain([0, Math.max(10, d3.max(data, d => d.value))])
    //       .range([margin.left, width - margin.right]);
      
    //   d3.select(yAxis)
    //       .call(d3.axisLeft(yScale))
    //       .attr('stroke-width', 2)
    //       .attr('font-family', 'Verdana')
    //       .selectAll('.tick line')
    //       .attr('font-size', 12);
      
    //   d3.select(xAxis)
    //       .call(d3.axisBottom(xScale))
    //       .attr('stroke-width', 2)
    //       .selectAll('.tick > text')
    //       .attr('y', 0)
    //       .attr('dy', '0.35em')
    //       .attr('dx', '-1em')
    //       .attr('text-anchor', 'end')
    //       .attr('font-family', 'Verdana')
    //       .attr('font-size', 12)
    //       .attr('transform', 'rotate(-90)')
    //       ;}

     $: {yScale = d3.scaleBand()
              .domain(filteredData.map(d => d.age))
              .range([margin.top, height - margin.bottom])
              .padding(0.1);

      xScale = d3.scaleLinear()
          .domain([0, Math.max(10, d3.max(filteredData, d => d.value))])
          .range([margin.left, width - margin.right]);
      
      d3.select(yAxis)
          .call(d3.axisLeft(yScale))
          .attr('stroke-width', 2)
          .attr('font-family', 'Verdana')
          .selectAll('.tick line')
          .attr('font-size', 12);
      
      d3.select(xAxis)
          .call(d3.axisBottom(xScale))
          .attr('stroke-width', 2)
          .selectAll('.tick > text')
          .attr('y', 0)
          .attr('dy', '0.35em')
          .attr('dx', '-1em')
          .attr('text-anchor', 'end')
          .attr('font-family', 'Verdana')
          .attr('font-size', 12)
          .attr('transform', 'rotate(-90)')
          ;}

    let highlightedBar = null;

    function handleHover(i) {
        highlightedBar = i;
    }

    function handleLeave() {
        highlightedBar = null;
    } 

</script>

<main>
  <h1>Anxiety Prevalence By Age</h1>
  
  <input bind:value={country} type="text" />
  <input type="number" bind:value={year} />

  <button on:click={search}>Search</button>

  <svg {width}{height}>
    <!-- {#if index === 1}  -->
      <g class='bars'>
        {#each filteredData as d, i}
          <rect	
            x={margin.left}
            y={yScale(d.age)}
            width={xScale(d.value) - margin.left}
            height={yScale.bandwidth()}
            fill='#e3c466'
            class:highlighted={highlightedBar === i}
            on:mouseover={() => handleHover(i)}
            on:mouseout={() => handleLeave()}
          />
        {/each}
      </g>

      <g transform="translate(0, {height - margin.bottom})"
        bind:this={xAxis} />
        
      <g transform="translate({margin.left}, 0)"
      bind:this={yAxis} />

      <g class="axis-title">
        <text transform={`translate(${1000}, ${height - margin.bottom + 50})`} text-anchor="middle" font-family="Verdana">Individuals With Anxiety disorders (%)</text>
      </g>

      <g class="axis-title">
          <text transform={`translate(${margin.left - 70}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Age</text>    
      </g> 
    <!-- {/if} -->
  </svg>
</main>

<style>
  .highlighted {
        fill: #fce6a4;
    }
</style>
