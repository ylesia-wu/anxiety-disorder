<script>
  import Scroller from "@sveltejs/svelte-scroller";
  // import { fade } from 'svelte/transition';
  // import Age from "./Age.svelte";
  import LoadAge from "./LoadAge.svelte";
  import LoadGender from "./LoadGender.svelte";

  import CustomerIcon from './CustomerIcon.svelte';
  import LoadStudent from "./LoadStudent.svelte";

  import Quiz from './Quiz.svelte';
  import Timeline from "./Timeline.svelte";
  // import "@smastrom/flowbite/dist/flowbite.min.css";

  let count, index, offset, progress;

  let highlightedBar = null;

  function handleMouseover(event) {
    highlightedBar = event.detail.index;
  }

  function handleMouseleave() {
    highlightedBar = null;
  }

</script>

<!-- <div>
  <h1>Welcome to My Website</h1>
  <p>
      <CustomerIcon /> Customer Section
  </p>
</div> -->


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
  </div>

  <div class="foreground" slot="foreground">
    
    <section class="intro">
      <div class="intro-content">
        <h1 style="font-family: Verdana">Anxiety Disorder</h1>
        <p style="font-family: Verdana">Nowadays, anxiety is a normal part of our daily life. People worry about things such as study, work, money, or family issues. However, anxiety disorder involves more than temporary worry or fear, and it does not go away and even get worse over time. The symptoms can interfere with daily activities such as job performance, schoolwork, and relationships. So how can we self-test whether we have symptoms of anxiety disorder? First, let’s take a look at the trends in anxiety disorders in the United States.</p>
      </div>
    </section>

    <section>
      <LoadStudent {index}/>
    </section>
      
    <section>
      <LoadAge 
        {index}
        on:mouseover={handleMouseover}
        on:mouseleave={handleMouseleave}
      />
    </section>

    <section>
      <LoadGender 
      {index}
      on:mouseover={handleMouseover}
      on:mouseleave={handleMouseleave}
      />
    </section>
    
    <section>
      <h1>What are the signs and symptoms of anxiety?</h1>
    </section>
      
    <section>
      <Quiz/>
    </section>
      
    <section>
      <h1>Resources</h1>
    </section>
  </div>
</Scroller>

<style>
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: white solid 3px;
  }

  .foreground {
    width: 100%;
    height: auto;
    position: relative;
    outline: white solid 3px;
  }

  .intro {
    top: 0;
    left: 0;
    width: 100%;
    background-color: #d1e2f0;
    padding: 2rem;
  }

  .intro-content {
    max-width: 800px; 
    margin: 0 auto;
  } 

  section {
    height: 80vh;
    background-color: rgba(0, 0, 0, 0.0); /* 20% opaque */
    /* color: white; */
    outline: white 3px;
    text-align: center;
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }
</style>
