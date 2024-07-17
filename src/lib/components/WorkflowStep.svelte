<!-- src/lib/components/WorkflowStep.svelte -->
<script>
  export let stepNumber;
  export let title;
  export let description;
  export let icon;

  import { onMount } from 'svelte';
  import { gsap } from 'gsap';

  let step;

  onMount(() => {
    gsap.from(step, { opacity: 0, y: 50, duration: 1, delay: stepNumber * 0.3 });
  });
</script>

<div class="flex flex-col items-center p-6 bg-white shadow-lg rounded-lg m-4" bind:this={step}>
  <div class="text-4xl mb-4">{@html icon}</div>
  <h3 class="text-xl font-semibold mb-2">Step {stepNumber}: {title}</h3>
  <p class="text-center">{description}</p>
</div>

<style>
  :global(.flex) {
    flex: 1 1 300px;
  }
</style>
