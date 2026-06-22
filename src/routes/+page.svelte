<script lang="ts">
  import { onMount } from 'svelte';
  import Navbar   from '$lib/components/Navbar.svelte';
  import Hero     from '$lib/components/Hero.svelte';
  import About    from '$lib/components/About.svelte';
  import Skills   from '$lib/components/Skills.svelte';
  import Projects from '$lib/components/Projects.svelte';
  import Contact  from '$lib/components/Contact.svelte';
  import Footer   from '$lib/components/Footer.svelte';

  // Intersection observer for reveal animations
  onMount(() => {
    const revealEls = document.querySelectorAll('.reveal');
    const obs = new IntersectionObserver(
      (entries) => {
        entries.forEach(e => {
          if (e.isIntersecting) {
            e.target.classList.add('visible');
            obs.unobserve(e.target);
          }
        });
      },
      { threshold: 0.1 }
    );
    revealEls.forEach(el => obs.observe(el));
    return () => obs.disconnect();
  });
</script>

<svelte:head>
  <title>Sebastian Mazo — Desarrollador Full-Stack</title>
</svelte:head>

<!-- Noise overlay -->
<div class="noise" aria-hidden="true"></div>

<Navbar />
<main>
  <Hero />

  <!-- Divider -->
  <div class="section-divider" aria-hidden="true"></div>

  <About />

  <div class="section-divider" aria-hidden="true"></div>

  <Skills />

  <div class="section-divider" aria-hidden="true"></div>

  <Projects />

  <div class="section-divider" aria-hidden="true"></div>

  <Contact />
</main>
<Footer />

<style>
  main { position: relative; z-index: 1; }

  .noise {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 9999;
    opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
    background-repeat: repeat;
    background-size: 200px;
  }

  .section-divider {
    height: 1px;
    background: linear-gradient(to right, transparent, rgba(139,92,246,0.2), rgba(6,182,212,0.2), transparent);
    margin: 0 24px;
  }
</style>
