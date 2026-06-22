<script lang="ts">
  import { onMount } from 'svelte';

  let scrolled = $state(false);
  let mobileOpen = $state(false);

  const navLinks = [
    { href: '#about',    label: 'Sobre Mí'    },
    { href: '#skills',   label: 'Habilidades' },
    { href: '#projects', label: 'Proyectos'   },
    { href: '#contact',  label: 'Contacto'    },
  ];

  onMount(() => {
    const handleScroll = () => { scrolled = window.scrollY > 40; };
    window.addEventListener('scroll', handleScroll, { passive: true });
    return () => window.removeEventListener('scroll', handleScroll);
  });

  function closeMenu() { mobileOpen = false; }
</script>

<nav class:scrolled aria-label="Main navigation">
  <div class="nav-inner container">
    <!-- Logo -->
    <a href="#hero" class="nav-logo" onclick={closeMenu}>
      <span class="logo-bracket">&lt;</span>
      <span class="logo-name">SM</span>
      <span class="logo-bracket">/&gt;</span>
    </a>

    <!-- Desktop links -->
    <ul class="nav-links" role="list">
      {#each navLinks as { href, label }}
        <li>
          <a {href} class="nav-link">{label}</a>
        </li>
      {/each}
    </ul>

    <!-- Hamburger -->
    <button
      class="hamburger"
      class:open={mobileOpen}
      onclick={() => mobileOpen = !mobileOpen}
      aria-label="Toggle menu"
      aria-expanded={mobileOpen}
    >
      <span></span><span></span><span></span>
    </button>
  </div>

  <!-- Mobile drawer -->
  {#if mobileOpen}
    <div class="mobile-drawer glass" role="dialog" aria-modal="true">
      <ul role="list">
        {#each navLinks as { href, label }}
          <li>
            <a {href} class="mobile-link" onclick={closeMenu}>{label}</a>
          </li>
        {/each}
      </ul>
    </div>
  {/if}
</nav>

<style>
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 1000;
    padding: 20px 0;
    transition: all 0.4s var(--ease-in-out);
  }

  nav.scrolled {
    padding: 12px 0;
    background: rgba(8, 11, 18, 0.85);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
  }

  .nav-inner {
    display: flex;
    align-items: center;
    gap: 32px;
  }

  /* Logo */
  .nav-logo {
    font-family: var(--font-mono);
    font-size: 1.3rem;
    font-weight: 700;
    display: flex;
    align-items: center;
    gap: 2px;
    transition: opacity 0.2s;
  }
  .nav-logo:hover { opacity: 0.8; }
  .logo-bracket { color: var(--accent-purple); }
  .logo-name { color: var(--text-primary); }

  /* Links */
  .nav-links {
    display: flex;
    align-items: center;
    gap: 4px;
    list-style: none;
    margin-left: auto;
  }

  .nav-link {
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--text-secondary);
    padding: 8px 14px;
    border-radius: var(--radius-sm);
    transition: color 0.2s, background 0.2s;
    position: relative;
  }
  .nav-link::after {
    content: '';
    position: absolute;
    bottom: 4px; left: 50%; right: 50%;
    height: 2px;
    background: var(--grad-purple-cyan);
    border-radius: 99px;
    transition: left 0.3s var(--ease-out-expo), right 0.3s var(--ease-out-expo);
  }
  .nav-link:hover {
    color: var(--text-primary);
    background: rgba(255,255,255,0.04);
  }
  .nav-link:hover::after { left: 14px; right: 14px; }

  /* CTA button */
  .nav-cta { margin-left: 8px; }

  /* Hamburger */
  .hamburger {
    display: none;
    flex-direction: column;
    gap: 5px;
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px;
    margin-left: auto;
  }
  .hamburger span {
    display: block;
    width: 24px;
    height: 2px;
    background: var(--text-primary);
    border-radius: 99px;
    transition: transform 0.3s var(--ease-out-expo), opacity 0.3s;
  }
  .hamburger.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
  .hamburger.open span:nth-child(2) { opacity: 0; }
  .hamburger.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

  /* Mobile drawer */
  .mobile-drawer {
    position: absolute;
    top: 100%; left: 16px; right: 16px;
    border-radius: var(--radius-md);
    overflow: hidden;
    animation: slideUp 0.3s var(--ease-out-expo);
  }
  .mobile-drawer ul {
    list-style: none;
    display: flex;
    flex-direction: column;
    padding: 16px;
    gap: 4px;
  }
  .mobile-link {
    display: block;
    font-size: 1rem;
    font-weight: 500;
    color: var(--text-secondary);
    padding: 12px 16px;
    border-radius: var(--radius-sm);
    transition: color 0.2s, background 0.2s;
  }
  .mobile-link:hover { color: var(--text-primary); background: rgba(255,255,255,0.05); }
  .mobile-cta {
    display: block;
    text-align: center;
    margin-top: 8px;
  }

  /* btn-primary reused */
  :global(.btn-primary) {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.9rem;
    font-weight: 600;
    color: #fff;
    background: var(--grad-purple-cyan);
    padding: 10px 24px;
    border-radius: 99px;
    border: none;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.2s, box-shadow 0.2s;
    position: relative;
    overflow: hidden;
  }
  :global(.btn-primary::before) {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(255,255,255,0.15), transparent);
    opacity: 0;
    transition: opacity 0.2s;
  }
  :global(.btn-primary:hover) {
    opacity: 0.92;
    transform: translateY(-2px);
    box-shadow: var(--glow-purple);
  }
  :global(.btn-primary:hover::before) { opacity: 1; }
  :global(.btn-primary:active) { transform: translateY(0); }

  :global(.btn-outline) {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-primary);
    background: transparent;
    padding: 10px 24px;
    border-radius: 99px;
    border: 1px solid var(--border-active);
    cursor: pointer;
    transition: background 0.2s, border-color 0.2s, transform 0.2s;
  }
  :global(.btn-outline:hover) {
    background: rgba(139,92,246,0.08);
    border-color: var(--accent-purple);
    transform: translateY(-2px);
  }

  @media (max-width: 768px) {
    .nav-links, .nav-cta { display: none; }
    .hamburger { display: flex; }
  }
</style>
