<script>
  import { onMount } from 'svelte';

  let container;

  const steps = [
    {
      stepNumber: 1,
      title: 'Initial Consultation',
      description: 'We discuss your goals and understand your vision.',
      icon: '<svg ...>...</svg>', // Replace with actual SVG or icon
    },
    {
      stepNumber: 2,
      title: 'Research and Planning',
      description: 'We analyze and plan the best approach to achieve your goals.',
      icon: '<svg ...>...</svg>',
    },
    {
      stepNumber: 3,
      title: 'Implementation',
      description: 'We execute the plan and bring your vision to life.',
      icon: '<svg ...>...</svg>',
    },
    {
      stepNumber: 4,
      title: 'Review and Feedback',
      description: 'We review the progress and make necessary adjustments.',
      icon: '<svg ...>...</svg>',
    },
    {
      stepNumber: 5,
      title: 'Final Delivery',
      description: 'We deliver the final product and ensure your satisfaction.',
      icon: '<svg ...>...</svg>',
    },
  ];

  onMount(async () => {
    console.log('onMount triggered in WorkflowCarousel.svelte');
    if (typeof window !== 'undefined') {
      console.log('Window is defined, proceeding with imports and initialization');

      try {
        const { default: Lenis } = await import('lenis');
        const { gsap } = await import('gsap');
        const { ScrollTrigger } = await import('gsap/ScrollTrigger');
        gsap.registerPlugin(ScrollTrigger);
        console.log('GSAP and ScrollTrigger imported and registered');

        const lenis = new Lenis({
          duration: 1.2,
          easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
          direction: 'vertical',
          smooth: true,
          smoothTouch: true
        });
        console.log('Lenis initialized');

        function raf(time) {
          lenis.raf(time);
          requestAnimationFrame(raf);
        }
        requestAnimationFrame(raf);
        console.log('Lenis animation frame started');

        gsap.to(container, {
          xPercent: -100 * (steps.length - 1),
          ease: 'none',
          scrollTrigger: {
            trigger: container,
            pin: true,
            scrub: 1,
            snap: 1 / (steps.length - 1),
            end: () => `+=${container.offsetWidth}`
          }
        });
        console.log('GSAP animation for carousel initialized');
      } catch (error) {
        console.error('Error during initialization', error);
      }
    }
  });
</script>

<style>
  .carousel-container {
    display: flex;
    width: 100vw;
    overflow: hidden;
  }
  .carousel-slide {
    flex: 0 0 100vw;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
  }
</style>

<div bind:this={container} class="carousel-container">
  {#each steps as step (step.stepNumber)}
    <div class="carousel-slide">
      <div class="card">
        <div class="text-4xl mb-4">{@html step.icon}</div>
        <h3 class="text-xl font-semibold mb-2">Step {step.stepNumber}: {step.title}</h3>
        <p class="text-center">{step.description}</p>
      </div>
    </div>
  {/each}
</div>
