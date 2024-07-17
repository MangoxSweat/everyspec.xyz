<script>
  import { onMount } from 'svelte';
  import '../global.css'; // Import the global stylesheet

  let lenis;

  onMount(async () => {
    console.log('onMount triggered in +layout.svelte');
    if (typeof window !== 'undefined') {
      console.log('Window is defined, proceeding with imports and initialization');
      
      try {
        const { default: Lenis } = await import('lenis');
        const { gsap } = await import('gsap');
        const { ScrollTrigger } = await import('gsap/ScrollTrigger');
        gsap.registerPlugin(ScrollTrigger);
        console.log('GSAP and ScrollTrigger imported and registered');

        lenis = new Lenis({
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

        const barbaWrapper = document.getElementById('barba-wrapper');
        const barbaContainer = document.getElementById('barba-container');
        console.log('barba-wrapper:', barbaWrapper);
        console.log('barba-container:', barbaContainer);

        if (barbaWrapper && barbaContainer) {
          const barba = await import('@barba/core');
          console.log('Barba.js imported:', barba);

          if (barba && barba.default) {
            console.log('Barba.js object and default property found:', barba.default);
            barba.default.init({
              transitions: [{
                name: 'fade',
                leave(data) {
                  console.log('Barba.js leave transition triggered', data);
                  return gsap.to(data.current.container, { opacity: 0 });
                },
                enter(data) {
                  console.log('Barba.js enter transition triggered', data);
                  return gsap.from(data.next.container, { opacity: 0 });
                }
              }]
            });
            console.log('Barba.js initialized');
          } else {
            console.error('Barba.js object or default property not found');
          }
        } else {
          console.error('Barba.js wrapper or container not found');
        }
      } catch (error) {
        console.error('Error during initialization', error);
      }
    }
  });
</script>

<main id="barba-wrapper" data-barba="wrapper">
  <header>
    <img src="/logo.png" alt="EverySpec Logo">
    <nav>
      <a href="#about">About</a>
      <a href="#workflow">Workflow</a>
      <a href="#works">Works</a>
      <a href="#">Blog</a> 
      <a href="#contact">Contact</a>
    </nav>
  </header>
  <div id="barba-container" data-barba="container" data-barba-namespace="home">
    <slot />
  </div>
</main>

<style>
  /* Ensure these elements have the correct styles to be recognized by Barba.js */
  #barba-wrapper {
    display: block;
  }
  #barba-container {
    display: block;
  }
</style>
