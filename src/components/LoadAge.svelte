<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import Age from './Age.svelte';

    let ageData = [];
    let loadDataComplete = false;

    export let index;

    onMount(async () => {
      const res = await fetch('anxiety-disorders-prevalence-by-age.csv');
      const csv = await res.text();

      await d3.csvParse(csv, (d, i, columns) => {
        ageData.push(
          {
          entity: d['Entity'], 
          year: +d['Year'],
          data: [
            {age: "5-14", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 5-14 years"]},
            {age: "15-19", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 15-19 years"]},
            {age: "20-24", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 20-24 years"]},
            {age: "25-29", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 25-29 years"]},
            {age: "30-34", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 30-34 years"]},
            {age: "35-39", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 35-39 years"]},
            {age: "40-44", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 40-44 years"]},
            {age: "45-49", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 45-49 years"]},
            {age: "50-54", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 50-54 years"]},
            {age: "55-59", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 55-59 years"]},
            {age: "60-64", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 60-64 years"]},
            {age: "65-69", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 65-69 years"]},
            {age: "70+", value: +d["Anxiety disorders (share of population) - Sex: Both - Age: 70+ years"]},
        ]});
      });
      ageData=ageData;
      loadDataComplete = true;
    });
</script>

<main>
    {#if loadDataComplete}
        <Age {ageData} {index}/>
    {:else}
        <p>Loading...</p>
    {/if}
</main>