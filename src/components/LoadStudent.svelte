<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';

    import Student from './Student.svelte';

    let studentData = [];

    let loadDataComplete = false;

    export let index;

    onMount(async () => {
      const res = await fetch('Student Mental health.csv');
      const csv = await res.text();

      await d3.csvParse(csv, (d, i, columns) => {
        // console.log(d);
        studentData.push(
          {
          age: +d['Age'], 
          gender: d['Choose your gender'],
          anxiety: d['Do you have Anxiety?']});
      });
      studentData=studentData;
      loadDataComplete = true;
    });
</script>

<main>
    {#if loadDataComplete}
        <Student {studentData} {index}/>
    {:else}
        <p>Loading...</p>
    {/if}
</main>