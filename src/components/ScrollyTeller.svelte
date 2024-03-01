<script>
  import Scroller from "@sveltejs/svelte-scroller";
  import Age from "./Age.svelte";

  let count, index, offset, progress;

  let highlightedBar = null;

  function handleMouseover(event) {
    highlightedBar = event.detail.index;
  }

  function handleMouseleave() {
    highlightedBar = null;
  }

</script>

<Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background">
    <!-- <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>offset in current section</p>
      <progress value={offset || 0} />

      <p>total progress</p>
      <progress value={progress || 0} />
    </div> -->
  </div>

  <div class="foreground" slot="foreground">
    <section>This is the first section.</section>
      <h1>Anxiety Disorder</h1>
      
    <section>This is the second section.</section>
      
    <section>This is the third section.</section>
    <Age 
      {index}
      on:mouseover={handleMouseover}
      on:mouseleave={handleMouseleave}/>
    <section>This is the fourth section.</section>
    <section>This is the fifth section.</section>
    <section>This is the six section.</section>

  </div>
</Scroller>

<style>
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: green solid 3px;
  }

  .foreground {
    width: 90%;
    margin: 0 auto;
    height: auto;
    position: relative;
    outline: gray solid 3px;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 0vh;
    background-color: rgba(0, 0, 0, 0.2); /* 20% opaque */
    /* color: white; */
    outline: gray 3px;
    text-align: center;
    max-width: 1300px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }
</style>
