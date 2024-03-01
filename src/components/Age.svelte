<script lang="ts">
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    export let index, width, height;

    let ageData = [];

    onMount(async () => {
      const res = await fetch('anxiety-disorders-prevalence-by-age.csv');
      const csv = await res.text();

      await d3.csvParse(csv, (d, i, columns) => {

        ageData.push(
          {
          entity: d['Entity'], 
          year: d['Year'],
          data: [
            {age: "5-14", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 5-14 years"]},
            {age: "15-19", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 15-19 years"]},
            {age: "20-24", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 20-24 years"]},
            {age: "25-29", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 25-29 years"]},
            {age: "30-34", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 30-34 years"]},
            {age: "35-39", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 35-39 years"]},
            {age: "40-44", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 40-44 years"]},
            {age: "45-49", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 45-49 years"]},
            {age: "50-54", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 50-54 years"]},
            {age: "55-59", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 55-59 years"]},
            {age: "60-64", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 60-64 years"]},
            {age: "65-69", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 65-69 years"]},
            {age: "70+", value: d["Anxiety disorders (share of population) - Sex: Both - Age: 70+ years"]},
        ]});
      });
    });
    ageData = ageData;
    

    // let name = "united";
    // let filteredData = ageData.filter(d => d.entity.toLowerCase().startsWith("united"));
//     for (let i = 0; i < ageData.length; i++) {
//     console.log(ageData[i].country);
// } 
    console.log(ageData);
    console.log(ageData[0]);
    // const ageGroups = [
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 5-14 years": "5-14"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 15-19 years": "15-19"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 20-24 years": "20-24"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 25-29 years": "25-29"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 30-34 years": "30-34"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 35-39 years": "35-39"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 40-44 years": "40-44"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 45-49 years": "45-49"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 50-54 years": "50-54"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 55-59 years": "55-59"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 60-64 years": "60-64"},
    //   {"Anxiety disorders (share of population) - Sex: Both - Age: 65-69 years": "65-69"}
    // ];

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
              .domain(data.map(d => d.age))
              .range([margin.top, height - margin.bottom])
              .padding(0.1);

      xScale = d3.scaleLinear()
          .domain([0, Math.max(10, d3.max(data, d => d.value))])
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
  
  <svg {width}{height}>
    {#if index === 1} 
      <g class='bars'>
        {#each data as d, i}
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
        <text transform={`translate(${1000}, ${height - margin.bottom + 50})`} text-anchor="middle" font-family="Verdana">Anxiety disorders (share of population)</text>
      </g>

      <g class="axis-title">
          <text transform={`translate(${margin.left - 70}, ${height / 2 - 60}) rotate(-90)`} text-anchor="middle" font-family="Verdana">Age</text>    
      </g> 
    {/if}
  </svg>
</main>

<style>
  .highlighted {
        fill: #fce6a4;
    }
</style>
