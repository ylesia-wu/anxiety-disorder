<script>
  import Scroller from "@sveltejs/svelte-scroller";
  // import { fade } from 'svelte/transition';
  import Intro from './Intro.svelte';
  import LoadStudent from "./LoadStudent.svelte";
  import LoadAge from "./LoadAge.svelte";
  import LoadGender from "./LoadGender.svelte";
  import Symptoms from "./Symptoms.svelte";
  import Quiz from './Quiz.svelte';
  import Timeline from "./Timeline.svelte";
  import Sources from "./Sources.svelte";
  // import "@smastrom/flowbite/dist/flowbite.min.css";

  let count, index, offset, progress;

  let highlightedBar = null;

  function handleMouseover(event) {
    highlightedBar = event.detail.index;
  }

  function handleMouseleave() {
    highlightedBar = null;
  }

  // grace added start

  // let currentIndex = 1;
  //   const totalSlides = 5; // Total number of slides

  //   function nextSlide() {
  //       if (currentIndex < totalSlides) {
  //           currentIndex++;
  //       }
  //   }

  //   function prevSlide() {
  //       if (currentIndex > 1) {
  //           currentIndex--;
  //       }
  //   }

  // fix?
  import { onMount } from 'svelte';

  let currentIndex = 1;
  const totalSlides = 5; // Total number of slides

  function nextSlide() {
    if (currentIndex < totalSlides) {
      currentIndex++;
    }
  }

  function prevSlide() {
    if (currentIndex > 1) {
      currentIndex--;
    }
  }

  
  let isOpen = false; // State to manage accordion open/close

  // table of content 
  const sections = [
    { id: 'section-1', title: 'Overview' },
    { id: 'section-2', title: 'The Difference Between Anxiety in Men vs. Women' },
    { id: 'section-3', title: 'Signs & Symptoms of Anxiety in Men' },
    { id: 'section-4', title: 'Causes of Anxiety in Men' },
    { id: 'section-5', title: 'Types of Anxiety Men Experience' },
    { id: 'section-6', title: 'Anxiety Treatment for Men' }
    // Add more sections as needed
  ];

  // Function to scroll to section
  function scrollToSection(event) {
    const sectionId = event.target.getAttribute('data-section');
    const section = document.querySelector(sectionId);
    if (section) {
      section.scrollIntoView({ behavior: 'smooth' });
    }
  }


  // grace added end

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
      <Intro/>
    </section>

    <!-- grace add table of content -->
    <!-- <nav class="table-of-contents">
      <h4>Table of Contents:</h4>
      <ul>
        <li><a href="#overview" data-section="#overview" on:click={scrollToSection}>Overview</a></li>
        <li><a href="#difference" data-section="#difference" on:click={scrollToSection}>The Difference Between Anxiety in Men vs. Women</a></li>
        <li><a href="#symptoms" data-section="#symptoms" on:click={scrollToSection}>Signs & Symptoms of Anxiety in Men</a></li>
        <li><a href="#causes" data-section="#causes" on:click={scrollToSection}>Causes of Anxiety in Men</a></li>
        <li><a href="#types" data-section="#types" on:click={scrollToSection}>Types of Anxiety Men Experience</a></li>
        <li><a href="#treatment" data-section="#treatment" on:click={scrollToSection}>Anxiety Treatment for Men</a></li>
      </ul>
    </nav> -->
    <!-- grace add table of content end -->

  <!-- grace added slick-slider -->
  <!-- <div class="card-carousel slick-initialized slick-slider">
    <button on:click={prevSlide} class="slick-prev slick-arrow" aria-label="Previous" type="button">Previous</button>
    <div class="slick-list draggable">
      <div class="slick-track" style="opacity: 1; width: {totalSlides * 370}px;">
        {#each Array.from({ length: totalSlides }) as _, index}
          <div class="solid-card slick-slide {index === currentIndex - 1 ? 'slick-current slick-active' : ''}" 
               style="background-color: rgb(255, 245, 204); width: 370px; position: relative; left: {-(index * 370)}px; top: 0px; z-index: {998 + (index === currentIndex - 1 ? 1 : 0)}; opacity: {index === currentIndex - 1 ? 1 : 0}; transition: opacity 500ms ease 0s;" 
               data-slick-index="{index}" 
               aria-hidden="{index !== currentIndex - 1}" 
               tabindex="{index !== currentIndex - 1 ? -1 : 0}">
            <h3 class="card-title"></h3>
            {#if index === 0}
              <p>Almost one-third of college students report having felt so depressed that they had trouble functioning.</p>
            {:else if index === 1}
              <p>More than 80% of college students feel overwhelmed by their workload.</p>
            {:else if index === 2}
              <p>The demand for counseling services is growing 5 times faster than average student enrollment.</p>
            {:else if index === 3}
              <p>Only 25% of students with a mental health problem seek help.</p>
            {:else if index === 4}
              <p>Anxiety continues to be the most common diagnosis of the students that seek services at university counseling centers.</p>
            {/if}
          </div>
        {/each}
      </div>
    </div>
    <button on:click={nextSlide} class="slick-next slick-arrow" aria-label="Next" type="button">Next</button>
    <div class="slider-count">
      <p><span id="current">{currentIndex}</span> / <span id="total">{totalSlides}</span></p>
    </div>
  </div> -->

    
  
 
  <!-- grace added end -->

    <section class="overview" style="margin-left: 15%; margin-right: 15%; margin-top: 5%; text-align: justify; font-family: Verdana; font-size: 18px;">
        <p>
          Anxiety disorders were first officially recognized by the American Psychiatric Association in 1980. Prior to this time those suffering in this way might receive a general diagnosis of “nerves” or “stress.” Generalized anxiety disorder (GAD) is a common disorder characterized by long-lasting anxiety that is not focused on any one object or situation. Those with generalized anxiety disorder experience non-specific persistent fear and worry and become overly concerned with everyday matters.
        </p>
    </section>

    <section style="background-color: #fffbed">
      <LoadStudent {index}/>
    </section>
      
    <section>
      <LoadAge 
        {index}
        on:mouseover={handleMouseover}
        on:mouseleave={handleMouseleave}
      />
    </section>

    <section class="gender" style="background-color: #fffbed">
      <LoadGender 
      {index}
      on:mouseover={handleMouseover}
      on:mouseleave={handleMouseleave}
      />
    </section>
    
    <section class="symptoms">
      <Symptoms/>
    </section>
      
    <section class="quiz" style="background-color: #fffbed">
      <Quiz/>
    </section>

    <!-- bottom line grace -->
    <div data-qa="ContentCalloutH2Title" class="
    flow-root
    p-[24px]
    my-[32px]
    bg-inform
    rounded-[12px]
    [&amp;>h2]:mt-0
    [&amp;>h2]:mb-[12px]
    [&amp;>h2]:font-header-m
    sm:[&amp;>h2]:font-header-l
    [&amp;>p:first-of-type]:mt-0
    [&amp;>ul:first-of-type]:mt-0
    [&amp;>ol:first-of-type]:mt-0
    [&amp;>*:last-child]:mb-0
    [&amp;>*:last-child>li:last-child]:mb-0
  ">
  <!-- <h2 class="styles__StyledText-pkg__sc-1pf85vx-0 sJghp re-text" color="$text-primary">The bottom line</h2>
  <span class="styles__StyledText-pkg__sc-1pf85vx-0 gZzaRP sc-487baef-1 cTRlKV re-text" color="$text-primary" id="bottom-line"></span>
  <div id="highlight-bottom-line" class="styles__StyledBox-pkg__sc-13oi5ev-0 kXAINL"></div>
  <p class="styles__StyledText-pkg__sc-1pf85vx-0 kCzgZL re-text" color="$text-primary">Anxiety disorders have many different symptoms, including physical symptoms and debilitating worry and fear. Anxiety disorders can affect people at any time in life. But they often affect people of different ages in different ways. Fortunately, there are effective treatments — including psychotherapy and medications. And healthcare providers tailor them for specific age groups and individuals.</p> -->
</div>


<!-- <div class="content-callout">
  <h2>The bottom line</h2>
  <p>Anxiety disorders have many different symptoms, including physical symptoms and debilitating worry and fear. Anxiety disorders can affect people at any time in life. But they often affect people of different ages in different ways. Fortunately, there are effective treatments — including psychotherapy and medications. And healthcare providers tailor them for specific age groups and individuals.</p>
  <p>You should always meet a health provider for diagnosing anxiety disorder. They will provide you with a psychological evaluation that involves discussing your thoughts, feelings and behavior. Anxiety disorders often occur along with other mental health problems which can make diagnosis more challenging. 
  </p>
  <p>The two main treatments for anxiety disorders are psychotherapy and medications. Psychotherapy is also known as talk therapy or psychological counseling. It involves working with a therapist to reduce your anxiety symptoms. Several types of medications are used to help relieve symptoms, depending on the type of anxiety disorder you have and whether you also have other mental or physical health issues. </p>
</div> -->

  <!-- grace end -->
      
    <section>
      <Sources/>
    </section>
  </div>


</Scroller>

<style>
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    outline: white solid 0px;
  }

  .foreground {
    width: 100%;
    height: auto;
    position: relative;
    outline: white solid 0px;
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
    height: 90vh;
    background-color: rgba(0, 0, 0, 0.0); /* 20% opaque */
    /* color: white; */
    outline: white 3px;
    text-align: center;
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
  }

  .overview {
    height: 30vh;
  }

  .gender {
    height: 100vh;
  }

  .symptoms {
    height: 50vh;
  }

  .quiz {
    height: 100vh;
  }

  .content-callout {
    padding: 24px;
    margin: 32px auto; /* Centers the block horizontally */
    background-color: #f0f4f8; /* Example background color */
    border-radius: 12px;
    font-family: Verdana, sans-serif;
    max-width: 800px; /* Sets the maximum width of the block */
    /* You can adjust the 800px to whatever width suits your design */
  }

  .content-callout h2 {
    margin-top: 0;
    margin-bottom: 12px;
    font-size: 20px; /* Example font size */
  }

  .content-callout p {
    margin-top: 0;
    /* Additional paragraph styling if needed */
  }

  /* .table-of-contents {
    position: relative;
    padding-left: 30px;
    font-family: Arial, sans-serif;
  }

  .table-of-contents:before {
    content: '';
    position: absolute;
    left: 0;
    top: 25px;
    bottom: 0;
    width: 2px;
    background-color: #000; /* Vertical line color */
  /* }

  .table-of-contents h4 {
    margin: 0;
    font-size: 18px;
    font-weight: normal;
  }

  .table-of-contents ul {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .table-of-contents li {
    margin-bottom: 16px;
    position: relative;
  }

  .table-of-contents li:before {
    content: '';
    position: absolute;
    left: -10px;
    top: 10px;
    height: 10px;
    width: 10px;
    border-radius: 50%;
    background-color: #000; /* Dot color */
  /* }

  .table-of-contents a {
    color: #000; /* Text color */
    /* text-decoration: none;
    line-height: 1.5;
    padding-left: 20px;
  } */

  /* .table-of-contents a:hover {
    text-decoration: underline;
  }

  .table-of-contents li:before {
    content: '';
    position: absolute;
    left: -10px;
    top: 10px;
    height: 10px;
    width: 10px;
    border-radius: 50%;
    background-color: #000; /* Dot color */
    /* transition: transform 0.3s ease; /* Smooth transition for dot */
  /* }

  .table-of-contents li:hover:before {
    transform: scale(1.5); /* Enlarge the dot */
    /* background-color: #d1e2f0; /* Color of dot on hover */
  /* }  */

  .intro {
    top: 0;
    left: 0;
    width: 100%;
    height: 80vh;
    background-image: url('yellow-gray.avif'); /* Update with the correct path */
    background-size: cover; /* Ensures that the image covers the entire area */
    background-position: center center; /* Centers the image */
    background-repeat: no-repeat; /* Prevents the image from repeating */
    padding: 2rem;
  }
</style>
