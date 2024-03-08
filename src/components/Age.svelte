<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';

  export let ageData, index, width, height;
  
  let country = "United States";
  let year = 2019;
  let filteredData = ageData.filter((d) => (d.entity === country) & (d.year === year))[0].data;

  function search() {
      filteredData = ageData.filter((d) => (d.entity === country) & (d.year === year))[0].data;
      console.log(filteredData);
  }

  const margin = {top: 90, right: 80, bottom: 180, left: 650};
  width = 1300;
  height = 680;

  let xScale;
  let yScale;
  let xAxis;
  let yAxis;

  $: {
      yScale = d3.scaleBand()
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
          .attr('transform', 'rotate(-90)');
  }

  let highlightedBar = null;

  function handleHover(i) {
      highlightedBar = i;
  }

  function handleLeave() {
      highlightedBar = null;
  } 

  let tooltipPt = null;
  let tooltipX = null;
  let tooltipY = null;

  function onPointerMove(event, i) {
      console.log('the function ran');
      tooltipX = xScale(filteredData[i].value) - 150;
      tooltipY = yScale(filteredData[i].age);
      tooltipPt = filteredData[i];
      
  }

  function onPointerLeave() {
      tooltipPt = null;
      tooltipX = null;
      tooltipY = null;
  }
</script>

<main>
<div style="position: relative;">
  <h1 style="font-family: Verdana">Anxiety Prevalence By Age</h1>
  <input bind:value={country} type="text" style="position: absolute; top: 100px; left: 800px; transform: translateX(-50%);" />
  <input type="number" bind:value={year} style="position: absolute; top: 100px; left: 955px; transform: translateX(-50%);" />
  <button on:click={search} style="position: absolute; top: 100px; left: 1070px; transform: translateX(-50%);">Search</button>

  <svg id="svgContainer" {width}{height} viewBox="0 0 {width} {height}" style="width: auto; max-height: 100%; margin-top: 100px;">
    <g class='bars'>
      {#each filteredData as d, i}
        <rect	
          x={margin.left}
          y={yScale(d.age) + 1.5}
          width={xScale(d.value) - margin.left - 3}
          height={yScale.bandwidth() - 3}
          fill='#e3c466'
          stroke='#3b3729'
          stroke-width='1.5' 
          class:highlighted={highlightedBar === i}
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
      <text transform={`translate(${950}, ${height - margin.bottom + 50})`} text-anchor="middle" font-family="Verdana">Share of Age Group With Anxiety Disorders (%)</text>
    </g>

    <g class="axis-title">
        <text transform={`translate(${margin.left - 70}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Age</text>    
    </g> 

    {#if tooltipPt}
          <g transform="translate({tooltipX},{tooltipY})">
              <rect x="-70" y="-20" width="140" height="55" fill="white" stroke="black"></rect>
              <text x="0" y="0" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">Age: {tooltipPt.age}</text>
              <text x="0" y="20" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">   {tooltipPt.value}%</text>
          </g>
      {/if}
</svg>
</div>
</main>

<style>
.highlighted {
      fill: #fce6a4;
  }
</style>
