<script lang="ts">
  import { onMount } from 'svelte';

  const skills = [
    {
      category: 'Frontend',
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="22" height="22"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>`,
      color: 'purple',
      items: [
        { name: 'Svelte / SvelteKit', level: 92 },
        { name: 'React / Next.js',    level: 88 },
        { name: 'TypeScript',         level: 90 },
        { name: 'CSS / Tailwind',     level: 85 },
      ],
    },
    {
      category: 'Backend',
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="22" height="22"><rect x="2" y="3" width="20" height="14" rx="2"/><path d="M8 21h8M12 17v4"/></svg>`,
      color: 'cyan',
      items: [
        { name: 'NestJS',           level: 90 },
        { name: 'Python / FastAPI', level: 80 },
        { name: 'Spring Boot',      level: 78 },
        { name: 'Node.js',          level: 88 },
      ],
    },
    {
      category: 'Mobile',
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="22" height="22"><rect x="5" y="2" width="14" height="20" rx="2"/><path d="M12 18h.01"/></svg>`,
      color: 'pink',
      items: [
        { name: 'Kotlin Multiplatform', level: 80 },
        { name: 'Jetpack Compose',      level: 78 },
        { name: 'React Native',         level: 72 },
      ],
    },
    {
      category: 'DevOps y Herramientas',
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" width="22" height="22"><circle cx="12" cy="12" r="3"/><path d="M12 1v4M12 19v4M4.22 4.22l2.83 2.83M16.95 16.95l2.83 2.83M1 12h4M19 12h4M4.22 19.78l2.83-2.83M16.95 7.05l2.83-2.83"/></svg>`,
      color: 'green',
      items: [
        { name: 'Docker / Compose', level: 82 },
        { name: 'Git / GitHub',     level: 92 },
        { name: 'CI/CD Pipelines',  level: 75 },
        { name: 'Vercel / Railway',  level: 88 },
      ],
    },
  ];

  const techBadges = [
    'Svelte', 'React', 'TypeScript', 'NestJS', 'Spring Boot',
    'Kotlin', 'PostgreSQL', 'MongoDB', 'Docker', 'Prisma',
    'Redis', 'Vite', 'Git',
  ];

  let sectionEl: HTMLElement;
  let visible = $state(false);

  onMount(() => {
    const obs = new IntersectionObserver(
      ([entry]) => { if (entry.isIntersecting) { visible = true; obs.disconnect(); } },
      { threshold: 0.1 }
    );
    obs.observe(sectionEl);
    return () => obs.disconnect();
  });

  function colorVar(c: string) {
    const map: Record<string, string> = {
      purple: 'var(--accent-purple)',
      cyan:   'var(--accent-cyan)',
      pink:   'var(--accent-pink)',
      green:  'var(--accent-green)',
    };
    return map[c] ?? 'var(--accent-purple)';
  }
</script>

<section id="skills" class="section-py" bind:this={sectionEl}>
  <div class="container">
    <!-- Header -->
    <div class="section-header" class:visible>
      <span class="section-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14">
          <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
        </svg>
        Experiencia
      </span>
      <h2 class="section-title">Habilidades y Tecnologías</h2>
      <p class="section-desc">
        Un conjunto de herramientas construido a través de proyectos del mundo real, desde interfaces elegantes hasta backends robustos y distribuidos.
      </p>
    </div>

    <!-- Skill cards -->
    <div class="skills-grid" class:visible>
      {#each skills as skill, i}
        <div
          class="skill-card glass"
          style="--card-color:{colorVar(skill.color)}; transition-delay:{i*0.1}s"
        >
          <div class="skill-card-header">
            <div class="skill-icon" style="color:{colorVar(skill.color)}; background:rgba({
              skill.color==='purple'?'139,92,246':
              skill.color==='cyan'?'6,182,212':
              skill.color==='pink'?'236,72,153':
              '16,185,129'
            },0.12)">
              {@html skill.icon}
            </div>
            <h3 class="skill-category">{skill.category}</h3>
          </div>

          <div class="skill-bars">
            {#each skill.items as item, j}
              <div class="skill-item" style="transition-delay:{(i*0.1)+(j*0.08)}s">
                <div class="skill-item-header">
                  <span class="skill-name">{item.name}</span>
                </div>
                <div class="skill-divider"></div>
              </div>
            {/each}
          </div>
        </div>
      {/each}
    </div>

    <!-- Tech badges -->
    <div class="badges-wrap" class:visible>
      <p class="badges-title">También tengo experiencia con</p>
      <div class="badges">
        {#each techBadges as badge, i}
          <span class="badge" style="animation-delay:{i*0.04}s">{badge}</span>
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  .section-header {
    max-width: 560px;
    margin-bottom: 64px;
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.7s var(--ease-out-expo), transform 0.7s var(--ease-out-expo);
  }
  .section-header.visible { opacity: 1; transform: none; }

  /* Grid */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 24px;
    margin-bottom: 60px;
  }
  .skills-grid .skill-card {
    opacity: 0;
    transform: translateY(24px);
    transition: opacity 0.6s var(--ease-out-expo), transform 0.6s var(--ease-out-expo);
  }
  .skills-grid.visible .skill-card {
    opacity: 1;
    transform: none;
  }

  /* Card */
  .skill-card {
    padding: 28px;
    border-radius: var(--radius-lg);
    position: relative;
    overflow: hidden;
    transition-property: opacity, transform, box-shadow, border-color !important;
    border: 1px solid var(--border) !important;
  }
  .skill-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: var(--card-color);
    opacity: 0.6;
  }
  .skill-card:hover {
    border-color: var(--card-color) !important;
    box-shadow: 0 0 32px rgba(0,0,0,0.3), 0 0 0 1px var(--card-color);
    transform: translateY(-4px) !important;
  }

  .skill-card-header {
    display: flex;
    align-items: center;
    gap: 14px;
    margin-bottom: 24px;
  }
  .skill-icon {
    width: 44px; height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: var(--radius-sm);
  }
  .skill-category {
    font-size: 1.05rem;
    font-weight: 700;
  }

  /* Bars */
  .skill-bars { display: flex; flex-direction: column; gap: 16px; }
  .skill-item { opacity: 0; transform: translateX(-8px); transition: opacity 0.5s, transform 0.5s; }
  .skills-grid.visible .skill-item { opacity: 1; transform: none; }

  .skill-item-header {
    display: flex;
    align-items: center;
    margin-bottom: 8px;
  }
  .skill-name { font-size: 0.875rem; color: var(--text-secondary); font-weight: 500; }

  .skill-divider {
    height: 2px;
    background: linear-gradient(90deg, var(--card-color), transparent);
    opacity: 0.5;
  }

  /* Badges */
  .badges-wrap {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.7s 0.4s var(--ease-out-expo), transform 0.7s 0.4s var(--ease-out-expo);
  }
  .badges-wrap.visible { opacity: 1; transform: none; }
  .badges-title {
    font-size: 0.85rem;
    color: var(--text-muted);
    font-weight: 500;
    margin-bottom: 16px;
    letter-spacing: 0.05em;
  }
  .badges {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  .badge {
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--text-secondary);
    padding: 6px 14px;
    border-radius: 99px;
    border: 1px solid var(--border);
    background: rgba(255,255,255,0.03);
    transition: color 0.2s, border-color 0.2s, background 0.2s, transform 0.2s;
    cursor: default;
  }
  .badge:hover {
    color: var(--accent-purple);
    border-color: rgba(139,92,246,0.4);
    background: rgba(139,92,246,0.06);
    transform: translateY(-2px);
  }
</style>
