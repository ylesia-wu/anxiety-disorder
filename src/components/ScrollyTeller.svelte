<script>
  import Scroller from "@sveltejs/svelte-scroller";
  // import Age from "./Age.svelte";
  import LoadAge from "./LoadAge.svelte";
  import LoadGender from "./LoadGender.svelte";

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
    <section>
      
      <h1>Anxiety Disorder</h1>

      <p>Nowadays, anxiety is a normal part of our daily life. People worry about things such as study, work, money, or family issues. However, anxiety disorder involves more than temporary worry or fear, and it does not go away and even get worse over time. The symptoms can interfere with daily activities such as job performance, schoolwork, and relationships. So how can we self-test whether we have symptoms of anxiety disorder? First, let’s take a look at the trends in anxiety disorders in the United States.</p>
    </section>
      
    <section>
      <LoadAge 
        on:mouseover={handleMouseover}
        on:mouseleave={handleMouseleave}
      />
      
    </section>

    <section>
      <LoadGender 
      />
    </section>
    
    <section>
      <h1>What are the signs and symptoms of anxiety?</h1>
    </section>
      
    <section>
      <h1>Self-test quiz</h1>
    </section>
      
    <section>
      <h1>Resources</h1>
    </section>
      
    <section>
      <h1>Prototype Write-up</h1>
      <p style="text-align: left"><strong> 1. What have you done so far?</strong></p>  
        
      <p style="text-align: justify">
        Our topic for the project is anxiety disorder, and we plan to show both information and statistics about anxiety to the viewer of our visualization. First of all, we have set up the GitHub repository with GitHub Actions Page. On the webpage, we have the basic scroller function set up and have outlined the structure of the entire webpage. In terms of visualization, we have finished one interactive visualization that shows the proportion of individuals diagnosed with anxiety disorder within different age groups. The viewer will be able to search the country and year of their interest and view the corresponding plot.
      </p>

      
      <p style="text-align: left"><strong> 2. What will be the most challenging of your project to design and why?</strong></p>
      <p style="text-align: justify">
        So far the most challenging part of our project was actually getting the data to load properly. We wanted to load our dataset all in one place and perform filtering, but we didn’t realize the importance of loading the data in a separate svelte file. We originally loaded the dataset in the same file that contains the plot elements. Although we tried to make the plot only show after the data finished loading, all our attempts failed, and only loading the data in a separate file worked. Our topic for the project is anxiety disorder, and we plan to show both information and statistics about anxiety to the viewer of our visualization. First of all, we have set up the GitHub repository with GitHub Actions Page. On the webpage, we have the basic scroller function set up and have outlined the structure of the entire webpage. In terms of visualization, we have finished one interactive visualization that shows the proportion of individuals diagnosed with anxiety disorder within different age groups. The viewer will be able to search the country and year of their interest and view the corresponding plot.
      </p>
      <p style="text-align: justify">
        For the rest of the project, I think the most challenging part of our project to design will be how to make the visualization both interesting and useful for the viewer instead of simply showing statistics and facts. After all, the purpose of the project is to explain our subject of focus, and we need to show, not tell. I think we will mainly focus on making the information relevant to the viewer. We are considering recreating one of the clinical tests for anxiety as part of our visualization for the viewer to self-access their level of anxiety. At this point, we are not sure how exactly to implement this test, and I expect this to be the most challenging part of our project.
      </p>
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
    width: 90%;
    margin: 0 auto;
    height: auto;
    position: relative;
    outline: white solid 3px;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 80vh;
    background-color: rgba(0, 0, 0, 0.0); /* 20% opaque */
    /* color: white; */
    outline: white 3px;
    text-align: center;
    max-width: 1300px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }
</style>
