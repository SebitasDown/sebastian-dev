<script lang="ts">
  import { onMount } from 'svelte';

  const projects = [
    {
      title: 'CoderBoost',
      desc: 'Plataforma educativa SPA para gestión de cursos, videos y talleres de programación con chat IA integrado (Gemini), comentarios en tiempo real y subida de contenido multimedia.',
      tags: ['JavaScript', 'Vite', 'Bootstrap 5', 'Cloudinary', 'Gemini AI', 'Svelte'],
      color: 'cyan',
      link: 'https://cb-front.vercel.app/',
      repo: 'https://github.com/SebitasDown/cb-front',
      images: ['/images/coderboost/Captura desde 2026-06-20 22-52-29.png', '/images/coderboost/Captura desde 2026-06-20 22-55-15.png'],
      device: 'desktop',
      featured: true,
      year: '2026',
    },
    {
      title: 'NutriLens',
      desc: 'Rastreador de nutrición móvil multiplataforma que aprovecha el reconocimiento de imágenes de IA para identificar alimentos y calcular macros en tiempo real.',
      tags: ['Kotlin Multiplatform', 'Jetpack Compose', 'Coil'],
      color: 'cyan',
      link: 'https://github.com/SebitasDown/NutriLens/releases/tag/NutriLens',
      repo: 'https://github.com/SebitasDown/NutriLens',
      images: ['/images/nutrilens/Nutrilens 1.webp', '/images/nutrilens/Nutrilens 2.webp', '/images/nutrilens/NutriLens 3.webp'],
      device: 'mobile',
      featured: true,
      year: '2026',
    },
    {
      title: 'NotCloud',
      desc: 'Una plataforma de toma de notas en tiempo real con asistente de escritura impulsado por IA, edición colaborativa y cifrado de extremo a extremo. Desarrollada con SvelteKit.',
      tags: ['SvelteKit', 'PostgreSQL', 'Groq AI'],
      color: 'purple',
      link: 'https://not-cloud-frontend.vercel.app/',
      repo: 'https://github.com/HexaCode-droid/NotCloud-Frontend',
      images: ['/images/notcloud/Captura desde 2026-06-20 19-32-47.png', '/images/notcloud/Captura desde 2026-06-20 19-32-58.png', '/images/notcloud/Captura desde 2026-06-20 19-33-07.png'],
      device: 'desktop',
      featured: true,
      year: '2026',
    },
    {
      title: 'NutriLens API',
      desc: 'API RESTful de gestión nutricional con análisis de comidas por IA, autenticación JWT y OAuth2, base de datos relacional y documentación interactiva con Swagger.',
      tags: ['Spring Boot', 'Spring Security', 'MySQL', 'MongoDB', 'Docker'],
      color: 'purple',
      link: 'https://nutrilens-0x37.onrender.com/swagger-ui/index.html',
      repo: 'https://github.com/SebitasDown/NutriLend',
      featured: false,
      year: '2026',
    },
    {
      title: 'NotCloud API',
      desc: 'API RESTful para la plataforma NotCloud con autenticación JWT, gestión de notas en tiempo real y edición colaborativa. Construida con NestJS y Prisma.',
      tags: ['NestJS', 'Prisma', 'PostgreSQL', 'JWT'],
      color: 'green',
      link: 'https://not-cloud-bakend.vercel.app/api',
      repo: 'https://github.com/HexaCode-droid/NotCloud-Bakend',
      featured: false,
      year: '2026',
    },
    {
      title: 'CoderBoost API',
      desc: 'API RESTful para plataforma educativa con gestión de usuarios, videos, categorías y ponentes, sistema de comentarios, búsqueda con IA (Gemini) y chat en tiempo real.',
      tags: ['Node.js', 'Express', 'MySQL', 'Cloudinary', 'Gemini AI', 'JWT'],
      color: 'pink',
      link: 'https://github.com/SebitasDown/cb-back-prueba',
      repo: 'https://github.com/SebitasDown/cb-back-prueba',
      featured: false,
      year: '2026',
    },
    {
      title: 'Dinno Gateway',
      desc: 'API Gateway para microservicios Dinno con Spring Cloud, descubrimiento de servicios Eureka, autenticación JWT y enrutamiento centralizado.',
      tags: ['Spring Cloud', 'Eureka', 'Docker', 'JWT'],
      color: 'green',
      link: 'https://dinno-gateway.onrender.com/',
      repo: 'https://github.com/SebitasDown/Dinno-Gateway',
      featured: false,
      year: '2025',
    },
    {
      title: 'Dinno Pocket',
      desc: 'Microservicio de gestión financiera personal con seguimiento de ingresos/gastos, categorización inteligente y generación de reportes.',
      tags: ['Spring Boot', 'PostgreSQL', 'Docker', 'Eureka'],
      color: 'pink',
      link: 'https://dinno-pocket.onrender.com/swagger-ui/index.html#/',
      repo: 'https://github.com/SebitasDown/Dinno-Pocket',
      featured: false,
      year: '2025',
    },
    {
      title: 'Dinno Auth',
      desc: 'Microservicio de autenticación y autorización para el ecosistema Dinno con Spring Security, JWT, OAuth2 y gestión de sesiones.',
      tags: ['Spring Boot', 'Spring Security', 'JWT', 'Docker'],
      color: 'purple',
      link: 'https://dinno-auth.onrender.com/swagger-ui/index.html#/',
      repo: 'https://github.com/SebitasDown/Dinno-Auth',
      featured: false,
      year: '2025',
    },
  ];

  const colorMap: Record<string, string> = {
    purple: 'var(--accent-purple)',
    cyan:   'var(--accent-cyan)',
    pink:   'var(--accent-pink)',
    green:  'var(--accent-green)',
  };

  let sectionEl: HTMLElement;
  let visible = $state(false);
  let activeFilter = $state('Todos');
  let selectedProject = $state<typeof projects[0] | null>(null);
  const filters = ['Todos', 'Destacados', 'Backend', 'Móvil', 'Frontend'];

  let filtered = $derived(
    activeFilter === 'Todos'      ? projects :
    activeFilter === 'Destacados' ? projects.filter(p => p.featured) :
    activeFilter === 'Backend'    ? projects.filter(p => p.tags.some(t => ['NestJS','Spring Boot','Ktor','Spring Cloud'].includes(t))) :
    activeFilter === 'Móvil'      ? projects.filter(p => p.tags.some(t => ['Kotlin Multiplatform','React Native'].includes(t))) :
    projects.filter(p => p.tags.some(t => ['SvelteKit','React','Svelte'].includes(t)))
  );

  function openModal(project: typeof projects[0]) {
    selectedProject = project;
    document.body.style.overflow = 'hidden';
  }
  function closeModal() {
    selectedProject = null;
    document.body.style.overflow = '';
  }

  onMount(() => {
    const obs = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) { visible = true; obs.disconnect(); } },
      { threshold: 0.05 }
    );
    obs.observe(sectionEl);
    const onKey = (e: KeyboardEvent) => { if (e.key === 'Escape') closeModal(); };
    window.addEventListener('keydown', onKey);
    return () => { obs.disconnect(); window.removeEventListener('keydown', onKey); };
  });
</script>

<section id="projects" class="section-py" bind:this={sectionEl}>
  <div class="container">
    <!-- Header -->
    <div class="section-header" class:visible>
      <span class="section-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14">
          <path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z"/>
        </svg>
        Work
      </span>
      <h2 class="section-title">Proyectos Destacados</h2>
      <p class="section-desc">
        Una selección de proyectos que he construido — desde aplicaciones en producción hasta side-projects que resuelven problemas reales.
      </p>
    </div>

    <!-- Filters -->
    <div class="filters" class:visible>
      {#each filters as f}
        <button
          class="filter-btn"
          class:active={activeFilter === f}
          onclick={() => activeFilter = f}
        >{f}</button>
      {/each}
    </div>

    <!-- Projects grid -->
    <div class="projects-grid">
      {#each filtered as project, i (project.title)}
        <article
          class="project-card glass"
          class:featured={project.featured}
          class:has-images={project.images && project.images.length > 0}
          style="--c:{colorMap[project.color]}; animation-delay:{i*0.08}s"
          class:visible
        >
          <!-- Top accent -->
          <div class="card-accent" style="background:{colorMap[project.color]}"></div>

          <!-- Screenshot teaser — click to open modal -->
          {#if project.images && project.images.length > 0}
            <button class="card-teaser" onclick={() => openModal(project)} aria-label="Ver capturas de {project.title}">
              <img src={project.images[0]} alt="" class="teaser-bg" aria-hidden="true" />
              <div class="teaser-overlay"></div>
              
              {#if project.device === 'desktop'}
                <div class="teaser-browsers">
                  {#each project.images.slice(0,2) as img, idx}
                    <div class="mini-browser" style="--idx:{idx}">
                      <div class="mini-browser-bar">
                        <span class="m-dot"></span><span class="m-dot"></span><span class="m-dot"></span>
                      </div>
                      <div class="mini-browser-screen">
                        <img src={img} alt="" />
                      </div>
                    </div>
                  {/each}
                </div>
              {:else}
                <div class="teaser-phones">
                  {#each project.images.slice(0,3) as img, idx}
                    <div class="mini-phone" style="--idx:{idx}">
                      <img src={img} alt="" />
                    </div>
                  {/each}
                </div>
              {/if}

              <span class="teaser-cta">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14">
                  {#if project.device === 'desktop'}
                    <rect x="2" y="3" width="20" height="14" rx="2" ry="2"/>
                    <line x1="8" y1="21" x2="16" y2="21"/>
                    <line x1="12" y1="17" x2="12" y2="21"/>
                  {:else}
                    <rect x="5" y="2" width="14" height="20" rx="2"/><path d="M12 18h.01"/>
                  {/if}
                </svg>
                Ver capturas de pantalla
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
              </span>
            </button>
          {/if}

          <!-- Header -->
          <div class="card-head">
            <div class="card-icon-wrap" style="color:{colorMap[project.color]}; background:rgba(0,0,0,0.3)">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="20" height="20">
                <path d="M22 19a2 2 0 0 1-2 2H4a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5l2 3h9a2 2 0 0 1 2 2z"/>
              </svg>
            </div>
            {#if project.featured}
              <span class="featured-badge">Destacado</span>
            {/if}
            <span class="year">{project.year}</span>
          </div>

          <!-- Body -->
          <h3 class="card-title">{project.title}</h3>
          <p class="card-desc">{project.desc}</p>

          <!-- Tags -->
          <div class="card-tags">
            {#each project.tags as tag}
              <span class="tag" style="border-color:rgba(255,255,255,0.08)">{tag}</span>
            {/each}
          </div>

          <!-- Actions -->
          <div class="card-actions">
            <a href={project.link} class="card-link" aria-label="Live demo">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="16" height="16">
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                <polyline points="15 3 21 3 21 9"/>
                <line x1="10" y1="14" x2="21" y2="3"/>
              </svg>
              Live Demo
            </a>
            <a href={project.repo} class="card-link" aria-label="Source code">
              <svg viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
                <path d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34-.46-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.87 1.52 2.34 1.07 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.92 0-1.11.38-2 1.03-2.71-.1-.25-.45-1.29.1-2.64 0 0 .84-.27 2.75 1.02.79-.22 1.65-.33 2.5-.33.85 0 1.71.11 2.5.33 1.91-1.29 2.75-1.02 2.75-1.02.55 1.35.2 2.39.1 2.64.65.71 1.03 1.6 1.03 2.71 0 3.82-2.34 4.66-4.57 4.91.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2z"/>
              </svg>
              Source
            </a>
          </div>
        </article>
      {/each}
    </div>
  </div>
</section>

<!-- ── Screenshot modal ── -->
{#if selectedProject}
  <!-- svelte-ignore a11y_click_events_have_key_events a11y_no_static_element_interactions -->
  <div class="modal-backdrop" onclick={closeModal}>
    <div class="modal-panel" class:is-desktop-modal={selectedProject.device === 'desktop'} onclick={(e) => e.stopPropagation()}>
      <!-- Close -->
      <button class="modal-close" onclick={closeModal} aria-label="Cerrar">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" width="20" height="20"><path d="M18 6 6 18M6 6l12 12"/></svg>
      </button>

      <!-- Header -->
      <div class="modal-header">
        <div>
          <p class="modal-label">Vista previa de la app</p>
          <h2 class="modal-title">{selectedProject.title}</h2>
          <p class="modal-desc">{selectedProject.desc}</p>
        </div>
        <div class="modal-actions">
          <a href={selectedProject.link} target="_blank" rel="noopener" class="btn-primary" style="font-size:0.85rem;padding:10px 20px">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" y1="14" x2="21" y2="3"/></svg>
            Demo
          </a>
          <a href={selectedProject.repo} target="_blank" rel="noopener" class="modal-repo-btn">
            <svg viewBox="0 0 24 24" fill="currentColor" width="16" height="16"><path d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34-.46-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.87 1.52 2.34 1.07 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.92 0-1.11.38-2 1.03-2.71-.1-.25-.45-1.29.1-2.64 0 0 .84-.27 2.75 1.02.79-.22 1.65-.33 2.5-.33.85 0 1.71.11 2.5.33 1.91-1.29 2.75-1.02 2.75-1.02.55 1.35.2 2.39.1 2.64.65.71 1.03 1.6 1.03 2.71 0 3.82-2.34 4.66-4.57 4.91.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2z"/></svg>
            Código
          </a>
        </div>
      </div>

      <!-- Tags -->
      <div class="modal-tags">
        {#each selectedProject.tags as tag}
          <span class="tag" style="border-color:rgba(255,255,255,0.1)">{tag}</span>
        {/each}
      </div>

      <!-- Phone or Browser mockups -->
      {#if selectedProject.device === 'desktop'}
        <div class="modal-browsers">
          {#each selectedProject.images ?? [] as img, idx}
            <div class="modal-browser" style="--idx:{idx}; --total:{selectedProject.images?.length ?? 1}">
              <div class="modal-browser-shell">
                <div class="modal-browser-bar">
                  <span class="b-dot red"></span>
                  <span class="b-dot yellow"></span>
                  <span class="b-dot green"></span>
                  <div class="browser-address">{selectedProject.title.toLowerCase()}.vercel.app</div>
                </div>
                <div class="modal-browser-screen">
                  <img src={img} alt="{selectedProject.title} screenshot {idx+1}" />
                </div>
              </div>
            </div>
          {/each}
        </div>
      {:else}
        <div class="modal-phones">
          {#each selectedProject.images ?? [] as img, idx}
            <div class="modal-phone" style="--idx:{idx}; --total:{selectedProject.images?.length ?? 1}">
              <div class="modal-phone-shell">
                <div class="modal-phone-notch"></div>
                <div class="modal-phone-screen">
                  <img src={img} alt="{selectedProject.title} screenshot {idx+1}" />
                </div>
                <div class="modal-phone-home"></div>
              </div>
            </div>
          {/each}
        </div>
      {/if}

    </div>
  </div>
{/if}

<style>
  .section-header {
    max-width: 560px;
    margin-bottom: 40px;
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s var(--ease-out-expo), transform 0.7s var(--ease-out-expo);
  }
  .section-header.visible { opacity: 1; transform: none; }

  /* Filters */
  .filters {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 40px;
    opacity: 0;
    transform: translateY(16px);
    transition: opacity 0.5s 0.2s, transform 0.5s 0.2s;
  }
  .filters.visible { opacity: 1; transform: none; }

  .filter-btn {
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--text-muted);
    padding: 8px 18px;
    border-radius: 99px;
    border: 1px solid var(--border);
    background: transparent;
    cursor: pointer;
    transition: all 0.2s;
  }
  .filter-btn:hover, .filter-btn.active {
    color: var(--text-primary);
    border-color: var(--accent-purple);
    background: rgba(139,92,246,0.1);
  }
  .filter-btn.active {
    background: rgba(139,92,246,0.15);
    box-shadow: 0 0 12px rgba(139,92,246,0.2);
  }

  /* Grid */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
    gap: 24px;
  }

  /* Card */
  .project-card {
    padding: 28px;
    border-radius: var(--radius-lg);
    position: relative;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    gap: 0;
    border: 1px solid var(--border) !important;
    opacity: 0;
    transform: translateY(24px) scale(0.98);
    transition:
      opacity 0.6s var(--ease-out-expo),
      transform 0.6s var(--ease-out-expo),
      border-color 0.3s,
      box-shadow 0.3s;
  }
  .project-card.visible {
    opacity: 1;
    transform: none;
  }
  .project-card:hover {
    border-color: var(--c) !important;
    box-shadow: 0 8px 40px rgba(0,0,0,0.4), 0 0 0 1px var(--c);
    transform: translateY(-6px) !important;
  }
  .project-card.featured {
    background: linear-gradient(160deg, rgba(139,92,246,0.04) 0%, transparent 60%);
  }

  .card-accent {
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    opacity: 0.7;
    transition: opacity 0.3s;
  }
  .project-card:hover .card-accent { opacity: 1; }

  /* ── Card teaser (click to open modal) ── */
  .card-teaser {
    position: relative;
    width: calc(100% + 56px);
    margin: -28px -28px 20px;
    height: 200px;
    overflow: hidden;
    border: none;
    border-radius: var(--radius-lg) var(--radius-lg) 0 0;
    cursor: pointer;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    padding-bottom: 16px;
    background: transparent;
  }
  .teaser-bg {
    position: absolute;
    inset: 0;
    width: 100%; height: 100%;
    object-fit: cover;
    object-position: top;
    filter: blur(6px) brightness(0.35);
    transform: scale(1.08);
    transition: filter 0.4s, transform 0.4s;
  }
  .card-teaser:hover .teaser-bg {
    filter: blur(3px) brightness(0.45);
    transform: scale(1.12);
  }
  .teaser-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, rgba(6,182,212,0.08) 0%, rgba(0,0,0,0.6) 100%);
  }
  .teaser-phones {
    position: absolute;
    inset: 0;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: 0;
    padding-bottom: 40px;
  }
  .teaser-browsers {
    position: absolute;
    inset: 0;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: 0;
    padding-bottom: 30px;
  }
  .mini-browser {
    width: 130px; height: 86px;
    border-radius: 6px;
    background: #0d0d14;
    border: 1.5px solid rgba(139,92,246,0.4);
    box-shadow: 0 10px 25px rgba(0,0,0,0.6);
    overflow: hidden;
    flex-shrink: 0;
    transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1);
    display: flex;
    flex-direction: column;
  }
  .mini-browser[style*="--idx:0"] { transform: perspective(400px) rotateX(10deg) rotateY(15deg) rotateZ(-2deg) translateX(24px) scale(0.9); z-index: 1; opacity: 0.8; }
  .mini-browser[style*="--idx:1"] { transform: perspective(400px) rotateX(10deg) rotateY(-5deg) translateY(-8px) scale(1); z-index: 2; }
  .card-teaser:hover .mini-browser[style*="--idx:0"] { transform: perspective(400px) rotateX(8deg) rotateY(10deg) rotateZ(-1deg) translateX(16px) scale(0.94); opacity: 0.9; }
  .card-teaser:hover .mini-browser[style*="--idx:1"] { transform: perspective(400px) rotateX(8deg) rotateY(-2deg) translateY(-14px) scale(1.04); }
  
  .mini-browser-bar {
    height: 8px;
    background: #141420;
    border-bottom: 1px solid rgba(255,255,255,0.06);
    display: flex;
    align-items: center;
    gap: 2px;
    padding: 0 4px;
    flex-shrink: 0;
  }
  .m-dot {
    width: 3px; height: 3px;
    border-radius: 50%;
    background: rgba(255,255,255,0.25);
  }
  .mini-browser-screen {
    flex: 1;
    overflow: hidden;
    background: #000;
  }
  .mini-browser-screen img {
    width: 100%; height: 100%;
    object-fit: cover;
    object-position: top;
    display: block;
  }
  .mini-phone {
    width: 58px; height: 104px;
    border-radius: 12px;
    background: #0d0d14;
    border: 1.5px solid rgba(6,182,212,0.4);
    box-shadow: 0 8px 24px rgba(0,0,0,0.5);
    overflow: hidden;
    flex-shrink: 0;
    transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1);
  }
  .mini-phone[style*="--idx:0"] { transform: perspective(400px) rotateY(22deg) translateX(16px) scale(0.85); z-index: 1; }
  .mini-phone[style*="--idx:1"] { transform: perspective(400px) rotateY(0deg) translateY(-10px) scale(1);   z-index: 3; }
  .mini-phone[style*="--idx:2"] { transform: perspective(400px) rotateY(-22deg) translateX(-16px) scale(0.85); z-index: 1; }
  .card-teaser:hover .mini-phone[style*="--idx:0"] { transform: perspective(400px) rotateY(14deg) translateX(10px) scale(0.9); }
  .card-teaser:hover .mini-phone[style*="--idx:1"] { transform: perspective(400px) rotateY(0deg) translateY(-16px) scale(1.05); }
  .card-teaser:hover .mini-phone[style*="--idx:2"] { transform: perspective(400px) rotateY(-14deg) translateX(-10px) scale(0.9); }
  .mini-phone img { width: 100%; height: 100%; object-fit: cover; object-position: top; display: block; }
  .teaser-cta {
    position: relative;
    z-index: 4;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(255,255,255,0.12);
    backdrop-filter: blur(12px);
    border: 1px solid rgba(255,255,255,0.2);
    color: #fff;
    font-size: 0.8rem;
    font-weight: 700;
    padding: 8px 16px;
    border-radius: 99px;
    letter-spacing: 0.02em;
    transition: background 0.2s, border-color 0.2s, transform 0.2s;
    white-space: nowrap;
  }
  .card-teaser:hover .teaser-cta {
    background: rgba(6,182,212,0.25);
    border-color: rgba(6,182,212,0.5);
    transform: translateY(-2px);
  }

  .card-head {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 20px;
  }
  .card-icon-wrap {
    width: 40px; height: 40px;
    border-radius: var(--radius-sm);
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid rgba(255,255,255,0.08);
  }
  .featured-badge {
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--accent-purple);
    border: 1px solid rgba(139,92,246,0.3);
    background: rgba(139,92,246,0.08);
    padding: 3px 10px;
    border-radius: 99px;
  }
  .year {
    margin-left: auto;
    font-size: 0.75rem;
    color: var(--text-muted);
    font-family: var(--font-mono);
  }

  .card-title {
    font-size: 1.2rem;
    font-weight: 800;
    margin-bottom: 10px;
    line-height: 1.3;
  }
  .card-desc {
    font-size: 0.9rem;
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 20px;
    flex: 1;
  }

  .card-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    margin-bottom: 24px;
  }
  .tag {
    font-size: 0.72rem;
    font-weight: 600;
    color: var(--text-muted);
    padding: 4px 10px;
    border-radius: 99px;
    border: 1px solid;
    background: rgba(255,255,255,0.02);
    font-family: var(--font-mono);
    transition: color 0.2s, border-color 0.2s;
  }
  .project-card:hover .tag {
    color: var(--text-secondary);
    border-color: rgba(255,255,255,0.14);
  }

  .card-actions {
    display: flex;
    gap: 16px;
    padding-top: 20px;
    border-top: 1px solid var(--border);
  }
  .card-link {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--text-secondary);
    transition: color 0.2s, transform 0.2s;
  }
  .card-link:hover {
    color: var(--c);
    transform: translateX(2px);
  }

  /* ── Modal ── */
  .modal-backdrop {
    position: fixed;
    inset: 0;
    z-index: 9000;
    background: rgba(0,0,0,0.75);
    backdrop-filter: blur(12px);
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    animation: fadeIn 0.25s ease;
  }
  @keyframes fadeIn { from { opacity: 0 } to { opacity: 1 } }

  .modal-panel {
    position: relative;
    width: 100%;
    max-width: 900px;
    max-height: 90vh;
    overflow-y: auto;
    background: #0f0f1a;
    border: 1px solid rgba(6,182,212,0.2);
    border-radius: 24px;
    padding: 40px;
    box-shadow: 0 40px 80px rgba(0,0,0,0.6), 0 0 0 1px rgba(6,182,212,0.1);
    animation: slideUp 0.3s cubic-bezier(0.34,1.56,0.64,1);
  }
  .modal-panel.is-desktop-modal {
    max-width: 1100px;
  }
  @keyframes slideUp { from { opacity:0; transform:translateY(40px) scale(0.96) } to { opacity:1; transform:none } }

  .modal-close {
    position: absolute;
    top: 20px; right: 20px;
    width: 40px; height: 40px;
    border-radius: 50%;
    border: 1px solid var(--border);
    background: rgba(255,255,255,0.04);
    color: var(--text-secondary);
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: background 0.2s, border-color 0.2s;
  }
  .modal-close:hover { background: rgba(255,255,255,0.1); border-color: var(--accent-cyan); color: #fff; }

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 24px;
    margin-bottom: 20px;
  }
  .modal-label {
    font-size: 0.75rem;
    font-weight: 700;
    color: var(--accent-cyan);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 6px;
  }
  .modal-title {
    font-size: 2rem;
    font-weight: 900;
    line-height: 1.1;
    margin-bottom: 10px;
  }
  .modal-desc {
    font-size: 0.9rem;
    color: var(--text-secondary);
    line-height: 1.7;
    max-width: 480px;
  }
  .modal-actions {
    display: flex;
    gap: 10px;
    flex-shrink: 0;
    flex-wrap: wrap;
    align-items: flex-start;
  }
  .modal-repo-btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--text-secondary);
    border: 1px solid var(--border);
    padding: 10px 20px;
    border-radius: 99px;
    transition: color 0.2s, border-color 0.2s;
  }
  .modal-repo-btn:hover { color: #fff; border-color: rgba(255,255,255,0.3); }

  .modal-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 36px;
  }

  /* Modal phone grid */
  .modal-phones {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: 16px;
    min-height: 380px;
    padding: 20px 0 0;
  }
  .modal-phone {
    flex-shrink: 0;
    transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1);
  }
  .modal-phone[style*="--idx:0"] { transform: perspective(800px) rotateY(20deg)  translateX(24px) scale(0.88); z-index: 1; }
  .modal-phone[style*="--idx:1"] { transform: perspective(800px) rotateY(0deg)   translateY(-20px) scale(1);   z-index: 3; }
  .modal-phone[style*="--idx:2"] { transform: perspective(800px) rotateY(-20deg) translateX(-24px) scale(0.88); z-index: 1; }
  .modal-phones:hover .modal-phone[style*="--idx:0"] { transform: perspective(800px) rotateY(12deg)  translateX(14px) scale(0.93); }
  .modal-phones:hover .modal-phone[style*="--idx:1"] { transform: perspective(800px) rotateY(0deg)   translateY(-28px) scale(1.04); }
  .modal-phones:hover .modal-phone[style*="--idx:2"] { transform: perspective(800px) rotateY(-12deg) translateX(-14px) scale(0.93); }

  .modal-phone-shell {
    width: 180px; height: 360px;
    border-radius: 34px;
    background: #0d0d14;
    border: 2px solid rgba(6,182,212,0.4);
    box-shadow: 0 0 0 1px rgba(255,255,255,0.06), 0 30px 60px rgba(0,0,0,0.6);
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 14px 8px 12px;
    gap: 6px;
  }
  .modal-phone-notch {
    width: 56px; height: 10px;
    background: #0d0d14;
    border-radius: 99px;
    border: 1px solid rgba(255,255,255,0.08);
    flex-shrink: 0;
  }
  .modal-phone-screen {
    flex: 1;
    width: 100%;
    border-radius: 18px;
    overflow: hidden;
    background: #000;
  }
  .modal-phone-screen img {
    width: 100%; height: 100%;
    object-fit: cover;
    object-position: top;
    display: block;
  }
  .modal-phone-home {
    width: 60px; height: 5px;
    background: rgba(255,255,255,0.15);
    border-radius: 99px;
    flex-shrink: 0;
  }

  /* Modal browser grid */
  .modal-browsers {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    gap: 16px;
    min-height: 380px;
    padding: 20px 0 0;
    position: relative;
  }
  .modal-browser {
    flex-shrink: 0;
    transition: transform 0.4s cubic-bezier(0.34,1.56,0.64,1);
  }
  .modal-browser[style*="--idx:0"] { transform: perspective(1000px) rotateX(12deg) rotateY(18deg) rotateZ(-3deg) translateX(36px) scale(0.85); z-index: 1; opacity: 0.75; }
  .modal-browser[style*="--idx:1"] { transform: perspective(1000px) rotateX(8deg) rotateY(-5deg) translateY(-20px) scale(1); z-index: 3; }
  .modal-browser[style*="--idx:2"] { transform: perspective(1000px) rotateX(12deg) rotateY(-18deg) rotateZ(3deg) translateX(-36px) scale(0.85); z-index: 1; opacity: 0.75; }
  
  .modal-browsers:hover .modal-browser[style*="--idx:0"] { transform: perspective(1000px) rotateX(8deg) rotateY(12deg) rotateZ(-1deg) translateX(20px) scale(0.9); opacity: 0.9; }
  .modal-browsers:hover .modal-browser[style*="--idx:1"] { transform: perspective(1000px) rotateX(4deg) rotateY(-2deg) translateY(-28px) scale(1.03); }
  .modal-browsers:hover .modal-browser[style*="--idx:2"] { transform: perspective(1000px) rotateX(8deg) rotateY(-12deg) rotateZ(1deg) translateX(-20px) scale(0.9); opacity: 0.9; }

  .modal-browser-shell {
    width: 340px;
    height: 240px;
    border-radius: 12px;
    background: #0d0d14;
    border: 2px solid rgba(139,92,246,0.4);
    box-shadow: 0 0 0 1px rgba(255,255,255,0.06), 0 30px 60px rgba(0,0,0,0.6);
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }
  .modal-browser-bar {
    height: 22px;
    background: #141420;
    border-bottom: 1px solid rgba(255,255,255,0.06);
    display: flex;
    align-items: center;
    gap: 5px;
    padding: 0 10px;
    flex-shrink: 0;
    position: relative;
  }
  .b-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
  }
  .b-dot.red { background: #ff5f56; }
  .b-dot.yellow { background: #ffbd2e; }
  .b-dot.green { background: #27c93f; }
  
  .browser-address {
    font-size: 0.65rem;
    font-family: var(--font-mono);
    color: var(--text-muted);
    background: rgba(0,0,0,0.25);
    padding: 2px 12px;
    border-radius: 4px;
    margin-left: 12px;
    border: 1px solid rgba(255,255,255,0.03);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 180px;
  }
  .modal-browser-screen {
    flex: 1;
    overflow: hidden;
    background: #000;
  }
  .modal-browser-screen img {
    width: 100%; height: 100%;
    object-fit: cover;
    object-position: top;
    display: block;
  }

  @media (max-width: 640px) {
    .projects-grid { grid-template-columns: 1fr; }
    .modal-panel { padding: 24px 16px; }
    .modal-header { flex-direction: column; }
    .modal-title { font-size: 1.5rem; }
    .modal-phone-shell { width: 130px; height: 260px; }
    .modal-browser-shell { width: 220px; height: 160px; }
    .modal-browsers { min-height: 240px; }
  }
</style>
