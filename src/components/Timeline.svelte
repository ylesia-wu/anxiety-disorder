<script>
    import { Timeline, TimelineItem } from 'flowbite-svelte';
    import { CalendarWeekSolid } from 'flowbite-svelte-icons';
  </script>
  
<Timeline order="horizontal">
    <TimelineItem title="Flowbite Library v1.0.0" date="Released on December 2nd, 2021">
        <svelte:fragment slot="icon">
        <div class="flex items-center">
            <div class="flex z-10 justify-center items-center w-6 h-6 bg-primary-200 rounded-full ring-0 ring-white dark:bg-primary-900 sm:ring-8 dark:ring-gray-900 shrink-0">
            <CalendarWeekSolid class="w-3 h-3 text-primary-600 dark:text-primary-400" />
            </div>
            <div class="hidden sm:flex w-full bg-gray-200 h-0.5 dark:bg-gray-700" />
        </div>
        </svelte:fragment>
        <p class="text-base font-normal text-gray-500 dark:text-gray-400">Get started with dozens of web components and interactive elements.</p>
    </TimelineItem>
</Timeline>