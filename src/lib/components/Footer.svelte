<script lang="ts">
  const year = new Date().getFullYear();
</script>

<footer class="footer">
  <div class="footer-glow" aria-hidden="true"></div>
  <div class="container footer-inner">
    <!-- Logo -->
    <a href="#hero" class="footer-logo">
      <span class="logo-bracket">&lt;</span>
      <span>Sebastian Mazo</span>
      <span class="logo-bracket">/&gt;</span>
    </a>

    <!-- Quick links -->
    <nav aria-label="Footer navigation">
      <ul class="footer-links">
        <li><a href="#about">Sobre Mí</a></li>
        <li><a href="#skills">Habilidades</a></li>
        <li><a href="#projects">Proyectos</a></li>
        <li><a href="#contact">Contacto</a></li>
      </ul>
    </nav>

    <!-- Copy -->
    <p class="footer-copy">
      {year} Sebastian Mazo
    </p>
  </div>
</footer>

<style>
  .footer {
    position: relative;
    border-top: 1px solid var(--border);
    padding: 40px 0;
    overflow: hidden;
  }
  .footer-glow {
    position: absolute;
    top: 0; left: 50%; transform: translateX(-50%);
    width: 600px; height: 1px;
    background: var(--grad-purple-cyan);
    filter: blur(1px);
    opacity: 0.5;
  }

  .footer-inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 24px;
  }

  .footer-logo {
    font-family: var(--font-mono);
    font-weight: 700;
    font-size: 1rem;
    color: var(--text-secondary);
    display: flex;
    align-items: center;
    gap: 2px;
    transition: color 0.2s;
  }
  .footer-logo:hover { color: var(--text-primary); }
  .logo-bracket { color: var(--accent-purple); }

  .footer-links {
    display: flex;
    gap: 24px;
    list-style: none;
  }
  .footer-links a {
    font-size: 0.875rem;
    color: var(--text-muted);
    transition: color 0.2s;
  }
  .footer-links a:hover { color: var(--text-primary); }

  .footer-copy {
    font-size: 0.82rem;
    color: var(--text-muted);
  }

  @media (max-width: 640px) {
    .footer-inner { flex-direction: column; text-align: center; }
    .footer-links { flex-wrap: wrap; justify-content: center; }
  }
</style>
