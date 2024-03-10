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

  const margin = {top: 90, right: 30, bottom: 180, left: 100};
  width = 650;
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

<main style="display: flex; margin-left: 60px; margin-right: 10px;">
  <div style="flex: 1; margin-right: 30px;">
      <h1 style="font-family: Futura; font-size: 34px; margin-bottom: 20px; line-height: 3; text-align: left;">Not Just Young Adults ...</h1>
      <div style="width: 100%; font-family: Verdana; font-size: 18px; text-align: justify; line-height: 1.6;">
        <p>Anxiety is a pervasive issue that afflicts not just students but also a significant portion of the middle-aged population, who grapple with severe forms of this debilitating condition. A 2001-2003 U.S. survey showed over 32% of adults aged 18-60 had an anxiety disorder at some point, and those aged 30-49 had the highest rate reporting an anxiety disorder in the past year.</p>
        <p>The bar plot to the right presents anxiety disorder statistics according to Our World in Data. For the United States, data shows a consistent pattern of middle-aged groups having higher shares of the population that has anxiety.</p>
      </div>
      
      <div style="display: flex; justify-content: center; margin-top: 20px;">
          <input bind:value={country} type="text" class="form-control" style="margin-right: 5px;" />
          <input type="number" bind:value={year} class="form-control" style="margin-right: 5px;" />
          <button on:click={search} class="btn btn-light">Search</button>
      </div>
  </div>
  <svg id="svgContainer" {width}{height} viewBox="0 0 {width} {height}" style="width: auto; max-height: 100%; margin-top: 0px;">
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
                  on:mouseover={(event) => {handleHover(i); onPointerMove(event, i);}}
                  on:mouseout={(event) => {handleLeave(); onPointerLeave();}}
              />
          {/each}
      </g>
      <g transform="translate(0, {height - margin.bottom})" bind:this={xAxis} />
      <g transform="translate({margin.left}, 0)" bind:this={yAxis} />
      <g class="axis-title">
          <text transform={`translate(${360}, ${height - margin.bottom + 50})`} text-anchor="middle" font-family="Verdana">Share of Age Group With Anxiety Disorders (%)</text>
      </g>
      <g class="axis-title">
          <text transform={`translate(${margin.left - 70}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Age</text>    
      </g> 
      {#if tooltipPt}
          <g transform="translate({tooltipX},{tooltipY})">
              <rect x="-60" y="-20" width="120" height="55" fill="white" stroke="black"></rect>
              <text x="0" y="0" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">Age: {tooltipPt.age}</text>
              <text x="0" y="20" font-size="14" font-weight="regular" font-family="Arial, sans-serif" text-anchor="middle" dominant-baseline="central">   {tooltipPt.value}%</text>
          </g>
      {/if}
  </svg>
</main>




<style>
  .highlighted {
      fill: #fce6a4;
  }
</style>

