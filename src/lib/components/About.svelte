<script lang="ts">
  import { onMount } from 'svelte';

  let sectionEl: HTMLElement;
  let visible = $state(false);

  onMount(() => {
    const obs = new IntersectionObserver(
      ([e]) => { if (e.isIntersecting) { visible = true; obs.disconnect(); } },
      { threshold: 0.1 }
    );
    obs.observe(sectionEl);
    return () => obs.disconnect();
  });

  const timeline = [
    {
      year: '2026',
      title: 'Ingeniería de Software',
      company: 'Universidad EAFIT',
      desc: 'Iniciando estudios profesionales en Ingeniería de Software para profundizar en fundamentos académicos de ciencias de la computación, estructuras de datos y diseño de sistemas a gran escala.',
      color: 'purple',
    },
    {
      year: '2025 - 2026',
      title: 'Desarrollador Full-Stack',
      company: 'Freelance / Proyectos Propios',
      desc: 'Desarrollo de aplicaciones web y móviles full-stack para clientes y proyectos propios. Construcción de plataformas como NotCloud, NutriLens y sistemas de microservicios con SvelteKit, NestJS y Spring Boot.',
      color: 'cyan',
    },
    {
      year: '2025 - 2026',
      title: 'Entrenamiento Full-Stack',
      company: 'Riwi (Medellín)',
      desc: 'Formación intensiva inmersiva 100% presencial en desarrollo de software y tecnologías web modernas, uso avanzado de IA para codificación y desarrollo de habilidades socioemocionales e inglés técnico.',
      color: 'green',
    },
  ];

  const colorMap: Record<string, string> = {
    purple: 'var(--accent-purple)',
    cyan:   'var(--accent-cyan)',
    green:  'var(--accent-green)',
    pink:   'var(--accent-pink)',
  };
</script>

<section id="about" class="section-py" bind:this={sectionEl}>
  <div class="container about-layout">
    <!-- Left: Bio -->
    <div class="about-bio" class:visible>
      <span class="section-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14">
          <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/>
          <circle cx="12" cy="7" r="4"/>
        </svg>
        Sobre Mí
      </span>
      <h2 class="section-title">
        Creando Experiencias Digitales<br><span class="gradient-text">Línea por Línea</span>
      </h2>
      <p class="bio-text">
        Soy <strong>Sebastian Mazo</strong>, un apasionado desarrollador full-stack de Colombia 🇨🇴.
        Me especializo en construir aplicaciones eficientes y centradas en el usuario, uniendo un diseño hermoso con una ingeniería sólida.
      </p>
      <p class="bio-text" style="margin-top:16px">
        Cuando no estoy escribiendo código, estoy explorando nuevas tecnologías, colaborando en código abierto o
        diseñando mejores experiencias para desarrolladores. Creo que el gran software surge de la empatía: comprender
        al usuario y construir con propósito.
      </p>

      <!-- Highlighted traits -->
      <div class="traits">
        {#each ['Código Limpio', 'Obsesión por el Rendimiento', 'Enfoque en UX', 'Aprendizaje Continuo'] as trait}
          <span class="trait-pill">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" width="14" height="14">
              <polyline points="20 6 9 17 4 12"/>
            </svg>
            {trait}
          </span>
        {/each}
      </div>

    </div>

    <!-- Right: Timeline -->
    <div class="timeline-wrap" class:visible>
      <h3 class="timeline-heading">Experiencia y Educación</h3>
      <div class="timeline">
        {#each timeline as item, i}
          <div class="timeline-item" style="--item-color:{colorMap[item.color]}; transition-delay:{i*0.15}s">
            <div class="tl-dot"></div>
            <div class="tl-line"></div>
            <div class="tl-content glass">
              <span class="tl-year">{item.year}</span>
              <h4 class="tl-title">{item.title}</h4>
              <p class="tl-company">{item.company}</p>
              <p class="tl-desc">{item.desc}</p>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  .about-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: start;
  }

  /* Bio */
  .about-bio {
    opacity: 0;
    transform: translateX(-30px);
    transition: opacity 0.8s var(--ease-out-expo), transform 0.8s var(--ease-out-expo);
  }
  .about-bio.visible { opacity: 1; transform: none; }

  .bio-text {
    color: var(--text-secondary);
    line-height: 1.85;
    font-size: 0.95rem;
  }
  .bio-text strong { color: var(--text-primary); }

  .traits {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-top: 28px;
  }
  .trait-pill {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-size: 0.82rem;
    font-weight: 600;
    color: var(--text-secondary);
    padding: 7px 14px;
    border-radius: 99px;
    border: 1px solid var(--border);
    background: rgba(255,255,255,0.02);
    transition: all 0.2s;
  }
  .trait-pill:hover {
    color: var(--accent-purple);
    border-color: rgba(139,92,246,0.3);
    background: rgba(139,92,246,0.06);
  }
  .trait-pill svg { color: var(--accent-green); }

  /* Timeline */
  .timeline-wrap {
    opacity: 0;
    transform: translateX(30px);
    transition: opacity 0.8s 0.2s var(--ease-out-expo), transform 0.8s 0.2s var(--ease-out-expo);
  }
  .timeline-wrap.visible { opacity: 1; transform: none; }

  .timeline-heading {
    font-size: 1rem;
    font-weight: 700;
    color: var(--text-secondary);
    margin-bottom: 32px;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    font-size: 0.8rem;
  }

  .timeline { display: flex; flex-direction: column; gap: 0; }

  .timeline-item {
    display: grid;
    grid-template-columns: 16px 2px 1fr;
    gap: 0 16px;
    opacity: 0;
    transform: translateY(16px);
    transition: opacity 0.5s var(--ease-out-expo), transform 0.5s var(--ease-out-expo);
  }
  .timeline-wrap.visible .timeline-item { opacity: 1; transform: none; }

  .tl-dot {
    width: 16px; height: 16px;
    border-radius: 50%;
    background: var(--item-color);
    box-shadow: 0 0 12px var(--item-color);
    flex-shrink: 0;
    margin-top: 4px;
    position: relative;
    z-index: 1;
  }
  .tl-dot::before {
    content: '';
    position: absolute;
    inset: -3px;
    border-radius: 50%;
    border: 1px solid var(--item-color);
    opacity: 0.4;
  }

  .tl-line {
    width: 2px;
    background: linear-gradient(to bottom, var(--item-color), rgba(255,255,255,0.04));
    margin-top: 16px;
  }
  .timeline-item:last-child .tl-line { background: transparent; }

  .tl-content {
    padding: 20px;
    border-radius: var(--radius-md);
    margin-bottom: 16px;
    border: 1px solid var(--border) !important;
    transition: border-color 0.3s, box-shadow 0.3s, transform 0.3s;
  }
  .tl-content:hover {
    border-color: var(--item-color) !important;
    box-shadow: 0 0 24px rgba(0,0,0,0.3);
    transform: translateX(4px);
  }

  .tl-year {
    font-size: 0.72rem;
    font-weight: 700;
    font-family: var(--font-mono);
    color: var(--item-color);
    letter-spacing: 0.08em;
  }
  .tl-title {
    font-size: 1rem;
    font-weight: 700;
    margin: 6px 0 2px;
  }
  .tl-company {
    font-size: 0.82rem;
    color: var(--text-muted);
    margin-bottom: 10px;
    font-style: italic;
  }
  .tl-desc {
    font-size: 0.875rem;
    color: var(--text-secondary);
    line-height: 1.7;
  }

  @media (max-width: 900px) {
    .about-layout { grid-template-columns: 1fr; gap: 60px; }
    .about-bio { transform: translateY(30px) !important; }
    .timeline-wrap { transform: translateY(30px) !important; }
  }
</style>
