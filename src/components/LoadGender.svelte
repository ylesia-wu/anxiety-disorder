<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    import Gender from './Gender.svelte';

    let genderData = [];
    let loadDataComplete = false;

    export let index;

    onMount(async () => {
      const res = await fetch('anxiety-disorders-prevalence-males-vs-females.csv');
      const csv = await res.text();

      await d3.csvParse(csv, (d, i, columns) => {
        genderData.push(
          {
          entity: d['Entity'], 
          year: +d['Year'],
          data: [
            {female: +d['Anxiety disorders (share of population) - Sex: Female - Age: Age-standardized'], male: +d["Anxiety disorders (share of population) - Sex: Male - Age: All ages"]}
        ]});
      });
      genderData=genderData;
      genderData=genderData.filter((d) => (d.data[0].female !== 0) & (d.data[0].male !== 0));
    //   console.log(genderData);
      loadDataComplete = true;
    });
</script>

<main>
    {#if loadDataComplete}
        <Gender {genderData}{index} />
    {:else}
        <p>Loading...</p>
    {/if}
</main>