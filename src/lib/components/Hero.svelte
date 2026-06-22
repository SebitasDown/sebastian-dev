<script lang="ts">
  import { onMount } from 'svelte';

  let canvas: HTMLCanvasElement;
  let mounted = $state(false);
  let typedText = $state('');
  let showCursor = $state(true);

  const roles = [
    'Desarrollador Full-Stack',
    'Ingeniero de Apps Móviles',
    'Arquitecto Backend',
    'Entusiasta de UI/UX',
  ];

  onMount(() => {
    mounted = true;

    // Typewriter effect
    let roleIndex = 0;
    let charIndex = 0;
    let deleting = false;
    let pauseTimer = 0;

    function type() {
      const current = roles[roleIndex];
      if (!deleting && charIndex <= current.length) {
        typedText = current.slice(0, charIndex++);
        setTimeout(type, 80);
      } else if (!deleting && charIndex > current.length) {
        pauseTimer++;
        if (pauseTimer < 20) return setTimeout(type, 100);
        pauseTimer = 0;
        deleting = true;
        setTimeout(type, 60);
      } else if (deleting && charIndex > 0) {
        typedText = current.slice(0, --charIndex);
        setTimeout(type, 40);
      } else {
        deleting = false;
        roleIndex = (roleIndex + 1) % roles.length;
        setTimeout(type, 400);
      }
    }
    type();

    // Cursor blink
    setInterval(() => { showCursor = !showCursor; }, 530);

    // Particle canvas
    const ctx = canvas.getContext('2d')!;
    let W = canvas.width = window.innerWidth;
    let H = canvas.height = window.innerHeight;
    const particles: Particle[] = [];
    const N = 60;

    interface Particle {
      x: number; y: number;
      vx: number; vy: number;
      r: number; alpha: number;
      color: string;
    }

    const colors = ['#8b5cf6', '#06b6d4', '#ec4899'];

    for (let i = 0; i < N; i++) {
      particles.push({
        x: Math.random() * W, y: Math.random() * H,
        vx: (Math.random() - 0.5) * 0.3, vy: (Math.random() - 0.5) * 0.3,
        r: Math.random() * 2 + 0.5,
        alpha: Math.random() * 0.5 + 0.1,
        color: colors[Math.floor(Math.random() * colors.length)],
      });
    }

    let raf: number;
    function draw() {
      ctx.clearRect(0, 0, W, H);
      for (const p of particles) {
        p.x += p.vx; p.y += p.vy;
        if (p.x < 0) p.x = W; if (p.x > W) p.x = 0;
        if (p.y < 0) p.y = H; if (p.y > H) p.y = 0;
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
        ctx.fillStyle = p.color;
        ctx.globalAlpha = p.alpha;
        ctx.fill();
      }
      // draw lines between nearby particles
      ctx.globalAlpha = 1;
      for (let i = 0; i < N; i++) {
        for (let j = i + 1; j < N; j++) {
          const dx = particles[i].x - particles[j].x;
          const dy = particles[i].y - particles[j].y;
          const dist = Math.sqrt(dx*dx + dy*dy);
          if (dist < 100) {
            ctx.beginPath();
            ctx.moveTo(particles[i].x, particles[i].y);
            ctx.lineTo(particles[j].x, particles[j].y);
            ctx.strokeStyle = '#8b5cf6';
            ctx.globalAlpha = (1 - dist / 100) * 0.12;
            ctx.lineWidth = 1;
            ctx.stroke();
          }
        }
      }
      ctx.globalAlpha = 1;
      raf = requestAnimationFrame(draw);
    }
    draw();

    const onResize = () => {
      W = canvas.width = window.innerWidth;
      H = canvas.height = window.innerHeight;
    };
    window.addEventListener('resize', onResize);

    return () => {
      cancelAnimationFrame(raf);
      window.removeEventListener('resize', onResize);
    };
  });

  const stats = [
    { value: '10+',  label: 'Proyectos Creados'   },
    { value: '15+',  label: 'Tecnologías'         },
  ];
</script>

<section id="hero" class="hero" aria-label="Hero section">
  <!-- Particle canvas -->
  <canvas bind:this={canvas} class="particles-canvas" aria-hidden="true"></canvas>

  <!-- BG image -->
  <div class="hero-bg" aria-hidden="true">
    <img src="/hero_bg.png" alt="" />
  </div>

  <!-- Content -->
  <div class="container hero-content">
    <div class="hero-text" class:visible={mounted}>
      <div class="section-label reveal" style="margin-bottom:24px">
        <span class="dot"></span>
        Disponible para oportunidades
      </div>

      <h1 class="hero-heading reveal reveal-delay-1">
        Hola, soy <span class="gradient-text">Sebastian Mazo</span>
      </h1>

      <div class="typewriter-wrap reveal reveal-delay-2">
        <span class="typewriter-prefix">Desarrollo </span>
        <span class="typewriter-text">{typedText}<span class="cursor" class:show={showCursor}>|</span></span>
      </div>

      <p class="hero-desc reveal reveal-delay-3">
        Desarrollador apasionado por crear aplicaciones web y móviles de alto rendimiento.
        Convierto problemas complejos en soluciones digitales elegantes y escalables.
      </p>

      <div class="hero-actions reveal reveal-delay-4">
        <a href="#projects" class="btn-primary">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <path d="M3 3h7v7H3zM14 3h7v7h-7zM14 14h7v7h-7zM3 14h7v7H3z"/>
          </svg>
          Ver Proyectos
        </a>
        <a href="#contact" class="btn-outline">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
            <polyline points="22,6 12,13 2,6"/>
          </svg>
          Contáctame
        </a>
      </div>

      <!-- Social links -->
      <div class="hero-socials reveal reveal-delay-5">
        <a href="https://github.com/SebitasDown" target="_blank" rel="noopener" class="social-link" aria-label="GitHub">
          <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
            <path d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34-.46-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.87 1.52 2.34 1.07 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.92 0-1.11.38-2 1.03-2.71-.1-.25-.45-1.29.1-2.64 0 0 .84-.27 2.75 1.02.79-.22 1.65-.33 2.5-.33.85 0 1.71.11 2.5.33 1.91-1.29 2.75-1.02 2.75-1.02.55 1.35.2 2.39.1 2.64.65.71 1.03 1.6 1.03 2.71 0 3.82-2.34 4.66-4.57 4.91.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2z"/>
          </svg>
        </a>
        <a href="https://www.linkedin.com/in/sebastian-mazo-areiza-10a923395/" target="_blank" rel="noopener" class="social-link" aria-label="LinkedIn">
          <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
            <path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6zM2 9h4v12H2z"/>
            <circle cx="4" cy="4" r="2"/>
          </svg>
        </a>
        <a href="mailto:mazoosebas@gmail.com" class="social-link" aria-label="Email">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="20" height="20">
            <path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/>
            <polyline points="22,6 12,13 2,6"/>
          </svg>
        </a>
      </div>
    </div>

    <!-- Right visual -->
    <div class="hero-visual reveal reveal-delay-3" aria-hidden="true">
      <div class="avatar-ring">
        <div class="avatar-glow"></div>
        <div class="avatar-circle">
          <span class="avatar-initials">SM</span>
        </div>
        <!-- Orbiting dots -->
        <div class="orbit orbit-1">
          <div class="orbit-dot orbit-dot-purple"></div>
        </div>
        <div class="orbit orbit-2">
          <div class="orbit-dot orbit-dot-cyan"></div>
        </div>
        <div class="orbit orbit-3">
          <div class="orbit-dot orbit-dot-pink"></div>
        </div>
      </div>

      <!-- Stats -->
      <div class="stats-grid">
        {#each stats as { value, label }, i}
          <div class="stat-card glass" style="animation-delay:{i*0.1}s">
            <span class="stat-value gradient-text">{value}</span>
            <span class="stat-label">{label}</span>
          </div>
        {/each}
      </div>
    </div>
  </div>

  <!-- Scroll indicator -->
  <div class="scroll-indicator" aria-hidden="true">
    <div class="scroll-mouse">
      <div class="scroll-wheel"></div>
    </div>
    <span>Scroll</span>
  </div>
</section>

<style>
  /* Hero container */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    overflow: hidden;
  }

  /* Particle canvas */
  .particles-canvas {
    position: absolute;
    inset: 0;
    pointer-events: none;
    z-index: 1;
  }

  /* BG image */
  .hero-bg {
    position: absolute;
    inset: 0;
    z-index: 0;
  }
  .hero-bg img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.18;
  }

  /* Content */
  .hero-content {
    position: relative;
    z-index: 2;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: center;
    padding-top: 100px;
    padding-bottom: 60px;
  }

  .hero-text { max-width: 600px; }

  .dot {
    width: 8px; height: 8px;
    background: var(--accent-green);
    border-radius: 50%;
    animation: pulse-glow 2s ease-in-out infinite;
    box-shadow: 0 0 8px var(--accent-green);
  }

  /* Heading */
  .hero-heading {
    font-size: clamp(2.4rem, 5vw, 4rem);
    font-weight: 900;
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 24px;
  }

  /* Typewriter */
  .typewriter-wrap {
    font-size: clamp(1.2rem, 2.5vw, 1.6rem);
    font-weight: 600;
    margin-bottom: 24px;
    color: var(--text-secondary);
    font-family: var(--font-mono);
  }
  .typewriter-text {
    color: var(--accent-cyan);
  }
  .cursor {
    opacity: 0;
    color: var(--accent-purple);
    font-weight: 300;
    transition: none;
  }
  .cursor.show { opacity: 1; }

  /* Desc */
  .hero-desc {
    font-size: 1.05rem;
    color: var(--text-secondary);
    line-height: 1.8;
    margin-bottom: 36px;
    max-width: 500px;
  }

  /* Actions */
  .hero-actions {
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    margin-bottom: 32px;
  }

  /* Socials */
  .hero-socials {
    display: flex;
    gap: 12px;
  }
  .social-link {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 42px; height: 42px;
    border-radius: 50%;
    border: 1px solid var(--border);
    color: var(--text-secondary);
    transition: color 0.2s, border-color 0.2s, background 0.2s, transform 0.2s;
  }
  .social-link:hover {
    color: var(--accent-purple);
    border-color: var(--accent-purple);
    background: rgba(139,92,246,0.08);
    transform: translateY(-3px);
  }

  /* ── Right visual ── */
  .hero-visual {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 40px;
  }

  /* Avatar */
  .avatar-ring {
    position: relative;
    width: 260px; height: 260px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .avatar-glow {
    position: absolute;
    inset: -20px;
    background: radial-gradient(circle, rgba(139,92,246,0.3) 0%, transparent 70%);
    animation: pulse-glow 3s ease-in-out infinite;
    border-radius: 50%;
  }
  .avatar-circle {
    width: 180px; height: 180px;
    border-radius: 50%;
    background: linear-gradient(135deg, #1e1b4b 0%, #0f172a 100%);
    border: 2px solid rgba(139,92,246,0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    z-index: 2;
    box-shadow: 0 0 60px rgba(139,92,246,0.2), inset 0 0 40px rgba(139,92,246,0.05);
  }
  .avatar-initials {
    font-size: 3.5rem;
    font-weight: 900;
    letter-spacing: -0.02em;
    background: var(--grad-purple-cyan);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Orbits */
  .orbit {
    position: absolute;
    border-radius: 50%;
    border: 1px dashed rgba(255,255,255,0.08);
    display: flex;
    align-items: flex-start;
    justify-content: center;
  }
  .orbit-1 { width: 220px; height: 220px; animation: rotate-slow 8s linear infinite; }
  .orbit-2 { width: 260px; height: 260px; animation: rotate-slow 12s linear infinite reverse; }
  .orbit-3 { width: 300px; height: 300px; animation: rotate-slow 16s linear infinite; }

  .orbit-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
    margin-top: -5px;
    box-shadow: 0 0 12px currentColor;
  }
  .orbit-dot-purple { background: var(--accent-purple); color: var(--accent-purple); }
  .orbit-dot-cyan   { background: var(--accent-cyan);   color: var(--accent-cyan); }
  .orbit-dot-pink   { background: var(--accent-pink);   color: var(--accent-pink); }

  /* Stats */
  .stats-grid {
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .stat-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px 24px;
    border-radius: var(--radius-md);
    min-width: 110px;
    animation: float 4s ease-in-out infinite;
    transition: transform 0.3s, box-shadow 0.3s;
  }
  .stat-card:hover {
    transform: translateY(-6px);
    box-shadow: var(--glow-purple);
  }
  .stat-card:nth-child(2) { animation-delay: 0.7s; }
  .stat-card:nth-child(3) { animation-delay: 1.4s; }

  .stat-value {
    font-size: 1.8rem;
    font-weight: 800;
    letter-spacing: -0.02em;
  }
  .stat-label {
    font-size: 0.75rem;
    color: var(--text-muted);
    font-weight: 500;
    margin-top: 4px;
    text-align: center;
  }

  /* Scroll indicator */
  .scroll-indicator {
    position: absolute;
    bottom: 40px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    z-index: 2;
    animation: float 2s ease-in-out infinite;
  }
  .scroll-indicator span {
    font-size: 0.7rem;
    letter-spacing: 0.12em;
    color: var(--text-muted);
    text-transform: uppercase;
  }
  .scroll-mouse {
    width: 22px; height: 34px;
    border: 2px solid var(--text-muted);
    border-radius: 99px;
    display: flex;
    justify-content: center;
    padding-top: 6px;
  }
  .scroll-wheel {
    width: 3px; height: 6px;
    background: var(--accent-purple);
    border-radius: 99px;
    animation: float 1.5s ease-in-out infinite;
  }

  /* Reveal on mount */
  .hero-text.visible .reveal {
    animation: slideUp 0.7s var(--ease-out-expo) both;
  }
  .hero-text.visible .reveal-delay-1 { animation-delay: 0.1s; }
  .hero-text.visible .reveal-delay-2 { animation-delay: 0.2s; }
  .hero-text.visible .reveal-delay-3 { animation-delay: 0.3s; }
  .hero-text.visible .reveal-delay-4 { animation-delay: 0.4s; }
  .hero-text.visible .reveal-delay-5 { animation-delay: 0.5s; }

  .hero-visual.reveal {
    animation: slideUp 0.8s var(--ease-out-expo) 0.3s both;
  }

  /* Responsive */
  @media (max-width: 900px) {
    .hero-content {
      grid-template-columns: 1fr;
      gap: 60px;
      text-align: center;
    }
    .hero-desc { margin-inline: auto; }
    .hero-actions { justify-content: center; }
    .hero-socials { justify-content: center; }
    .hero-visual { order: -1; }
  }
</style>
