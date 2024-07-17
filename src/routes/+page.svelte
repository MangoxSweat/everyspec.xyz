<script>
  import WorkflowCarousel from '$lib/components/WorkflowCarousel.svelte';
  import Contact from '$lib/components/Contact.svelte';
  import About from '$lib/components/About.svelte';
</script>

<div class="relative">
  <!--<section class="hero" style="background-image: url('/banner16x9.png');">-->
  <section class="hero">
    <div>
      <h1>Welcome to EverySpec</h1>
      <p>Optimizing EverySpec of Your Digital Growth!</p>
      <a href="#contact">Get Started</a>
    </div>
  </section>

  <section id="about" class="section">
    <About />
  </section>

  <section id="workflow" class="section">
    <h2>Our Workflow</h2>
    <WorkflowCarousel />
  </section>

  <section id="contact" class="section bg-gray-200">
    <Contact />
  </section>

  <footer>
    <p>&copy; 2024 EverySpec. All rights reserved.</p>
  </footer>
</div>
