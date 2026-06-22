<script lang="ts">
  import { onMount } from 'svelte';

  let sectionEl: HTMLElement;
  let visible = $state(false);
  let sending = $state(false);
  let sent = $state(false);
  let error = $state('');

  let name = $state('');
  let email = $state('');
  let message = $state('');

  onMount(() => {
    const obs = new IntersectionObserver(
      ([e]) => { if (e.isIntersecting) { visible = true; obs.disconnect(); } },
      { threshold: 0.1 }
    );
    obs.observe(sectionEl);
    return () => obs.disconnect();
  });

  async function handleSubmit(e: Event) {
    e.preventDefault();
    if (!name || !email || !message) { error = 'Por favor, completa todos los campos.'; return; }
    error = '';
    sending = true;
    // Simulate async send
    await new Promise(r => setTimeout(r, 1500));
    sending = false;
    sent = true;
  }

  const socials = [
    {
      label: 'GitHub',
      handle: '@SebitasDown',
      href: 'https://github.com/SebitasDown',
      color: 'var(--text-primary)',
      icon: `<svg viewBox="0 0 24 24" fill="currentColor" width="22" height="22"><path d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34-.46-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.87 1.52 2.34 1.07 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.92 0-1.11.38-2 1.03-2.71-.1-.25-.45-1.29.1-2.64 0 0 .84-.27 2.75 1.02.79-.22 1.65-.33 2.5-.33.85 0 1.71.11 2.5.33 1.91-1.29 2.75-1.02 2.75-1.02.55 1.35.2 2.39.1 2.64.65.71 1.03 1.6 1.03 2.71 0 3.82-2.34 4.66-4.57 4.91.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2z"/></svg>`,
    },
    {
      label: 'LinkedIn',
      handle: 'Sebastian Mazo Areiza',
      href: 'https://www.linkedin.com/in/sebastian-mazo-areiza-10a923395/',
      color: '#0a66c2',
      icon: `<svg viewBox="0 0 24 24" fill="currentColor" width="22" height="22"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6zM2 9h4v12H2z"/><circle cx="4" cy="4" r="2"/></svg>`,
    },
    {
      label: 'Email',
      handle: 'mazoosebas@gmail.com',
      href: 'mailto:mazoosebas@gmail.com',
      color: 'var(--accent-purple)',
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="22" height="22"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`,
    },
  ];
</script>

<section id="contact" class="section-py contact-section" bind:this={sectionEl}>
  <!-- Background orbs -->
  <div class="orb orb-left" aria-hidden="true"></div>
  <div class="orb orb-right" aria-hidden="true"></div>

  <div class="container contact-layout">
    <!-- Left: Info -->
    <div class="contact-info" class:visible>
      <span class="section-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="14" height="14">
          <path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07A19.5 19.5 0 0 1 4.69 12 19.79 19.79 0 0 1 1.61 3.38 2 2 0 0 1 3.6 1.18h3a2 2 0 0 1 2 1.72c.127.96.361 1.903.7 2.81a2 2 0 0 1-.45 2.11L7.91 8.78a16 16 0 0 0 7.31 7.31l1.18-1.18a2 2 0 0 1 2.11-.45c.907.339 1.85.573 2.81.7A2 2 0 0 1 22.82 17z"/>
        </svg>
        Contacto
      </span>
      <h2 class="section-title">
        Construyamos Algo<br><span class="gradient-text-pink">Increíble Juntos</span>
      </h2>
      <p class="section-desc">
        Actualmente estoy abierto a nuevas oportunidades. Si tienes un proyecto en mente, alguna duda
        o simplemente quieres saludar, mi bandeja de entrada siempre está disponible.
      </p>

      <!-- Socials -->
      <div class="social-cards">
        {#each socials as s}
          <a href={s.href} target="_blank" rel="noopener" class="social-card glass">
            <span class="sc-icon" style="color:{s.color}">{@html s.icon}</span>
            <div>
              <span class="sc-label">{s.label}</span>
              <span class="sc-handle">{s.handle}</span>
            </div>
            <svg class="sc-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="16" height="16">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
          </a>
        {/each}
      </div>
    </div>

    <!-- Right: Form -->
    <div class="contact-form-wrap" class:visible>
      <div class="form-card glass">
        {#if sent}
          <div class="success-state">
            <div class="success-icon">
              <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" width="32" height="32">
                <polyline points="20 6 9 17 4 12"/>
              </svg>
            </div>
            <h3>¡Mensaje Enviado!</h3>
            <p>Gracias por escribirme. Me pondré en contacto contigo pronto.</p>
          </div>
        {:else}
          <h3 class="form-title">Enviar un Mensaje</h3>
          <form onsubmit={handleSubmit} novalidate>
            <div class="form-row">
              <div class="form-group">
                <label for="contact-name">Tu Nombre</label>
                <input
                  id="contact-name"
                  type="text"
                  placeholder="Juan Pérez"
                  bind:value={name}
                  required
                />
              </div>
              <div class="form-group">
                <label for="contact-email">Correo Electrónico</label>
                <input
                  id="contact-email"
                  type="email"
                  placeholder="juan@correo.com"
                  bind:value={email}
                  required
                />
              </div>
            </div>
            <div class="form-group">
              <label for="contact-message">Mensaje</label>
              <textarea
                id="contact-message"
                rows="5"
                placeholder="Cuéntame sobre tu proyecto o idea..."
                bind:value={message}
                required
              ></textarea>
            </div>
            {#if error}
              <p class="form-error">{error}</p>
            {/if}
            <button type="submit" class="btn-primary submit-btn" disabled={sending}>
              {#if sending}
                <svg class="spin" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="16" height="16">
                  <path d="M21 12a9 9 0 1 1-6.219-8.56"/>
                </svg>
                Enviando...
              {:else}
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" width="16" height="16">
                  <line x1="22" y1="2" x2="11" y2="13"/>
                  <polygon points="22 2 15 22 11 13 2 9 22 2"/>
                </svg>
                Enviar Mensaje
              {/if}
            </button>
          </form>
        {/if}
      </div>
    </div>
  </div>
</section>

<style>
  .contact-section {
    position: relative;
    overflow: hidden;
  }

  .orb {
    position: absolute;
    width: 400px; height: 400px;
    border-radius: 50%;
    pointer-events: none;
    filter: blur(80px);
    opacity: 0.12;
  }
  .orb-left  { left: -100px; bottom: -100px; background: var(--accent-purple); }
  .orb-right { right: -100px; top: -100px;   background: var(--accent-pink); }

  .contact-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 80px;
    align-items: start;
    position: relative;
    z-index: 1;
  }

  /* Info */
  .contact-info {
    opacity: 0;
    transform: translateX(-30px);
    transition: opacity 0.8s var(--ease-out-expo), transform 0.8s var(--ease-out-expo);
  }
  .contact-info.visible { opacity: 1; transform: none; }

  /* Social cards */
  .social-cards { display: flex; flex-direction: column; gap: 12px; margin-top: 40px; }
  .social-card {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 16px 20px;
    border-radius: var(--radius-md);
    border: 1px solid var(--border) !important;
    transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s;
    cursor: pointer;
  }
  .social-card:hover {
    border-color: var(--border-active) !important;
    transform: translateX(4px);
    box-shadow: var(--glow-purple);
  }
  .sc-icon { flex-shrink: 0; }
  .sc-label {
    display: block;
    font-size: 0.75rem;
    font-weight: 600;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }
  .sc-handle {
    display: block;
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-primary);
    margin-top: 2px;
  }
  .sc-arrow {
    margin-left: auto;
    color: var(--text-muted);
    flex-shrink: 0;
    transition: transform 0.2s, color 0.2s;
  }
  .social-card:hover .sc-arrow { transform: translateX(4px); color: var(--accent-purple); }

  /* Form */
  .contact-form-wrap {
    opacity: 0;
    transform: translateX(30px);
    transition: opacity 0.8s 0.2s var(--ease-out-expo), transform 0.8s 0.2s var(--ease-out-expo);
  }
  .contact-form-wrap.visible { opacity: 1; transform: none; }

  .form-card {
    padding: 36px;
    border-radius: var(--radius-xl);
    border: 1px solid var(--border) !important;
  }
  .form-title {
    font-size: 1.3rem;
    font-weight: 800;
    margin-bottom: 28px;
  }

  .form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
    margin-bottom: 16px;
  }
  label {
    font-size: 0.82rem;
    font-weight: 600;
    color: var(--text-secondary);
  }
  input, textarea {
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    color: var(--text-primary);
    font-family: var(--font-sans);
    font-size: 0.9rem;
    padding: 12px 16px;
    transition: border-color 0.2s, box-shadow 0.2s;
    outline: none;
    resize: vertical;
  }
  input::placeholder, textarea::placeholder { color: var(--text-muted); }
  input:focus, textarea:focus {
    border-color: var(--accent-purple);
    box-shadow: 0 0 0 3px rgba(139,92,246,0.12);
  }
  textarea { min-height: 130px; }

  .form-error {
    font-size: 0.82rem;
    color: var(--accent-pink);
    margin-bottom: 12px;
  }

  .submit-btn { width: 100%; justify-content: center; padding: 14px 24px; }

  .spin { animation: rotate-slow 1s linear infinite; }

  /* Success */
  .success-state {
    text-align: center;
    padding: 40px 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }
  .success-icon {
    width: 72px; height: 72px;
    border-radius: 50%;
    background: rgba(16,185,129,0.1);
    border: 2px solid var(--accent-green);
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--accent-green);
    animation: pulse-glow 2s ease-in-out infinite;
  }
  .success-state h3 { font-size: 1.5rem; font-weight: 800; }
  .success-state p  { color: var(--text-secondary); }

  @media (max-width: 900px) {
    .contact-layout { grid-template-columns: 1fr; gap: 60px; }
    .contact-info  { transform: translateY(30px) !important; }
    .contact-form-wrap { transform: translateY(30px) !important; }
    .form-row { grid-template-columns: 1fr; }
  }
</style>
