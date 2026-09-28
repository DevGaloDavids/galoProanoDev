<template>
  <footer id="contacto" class="footer">
    <div class="footer-container">
      
      <!-- Lado izquierdo: Título y llamada rápida -->
      <div class="footer-brand">
        <h3>¿Hablamos?</h3>
        <p>Abierto a nuevas oportunidades y colaboraciones.</p>
      </div>

      <!-- Centro: Enlaces sociales compactos con icono y texto -->
      <div class="social-links">
        <a href="https://www.linkedin.com/in/galodavid-proano/" target="_blank" rel="noopener" aria-label="LinkedIn">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"/><rect x="2" y="9" width="4" height="12"/><circle cx="4" cy="4" r="2"/></svg>
          <span>LinkedIn</span>
        </a>

        <a href="https://github.com/DevGaloDavids" target="_blank" rel="noopener" aria-label="GitHub">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"/></svg>
          <span>GitHub</span>
        </a>

        <a href="mailto:galodavid.proano@gmail.com" aria-label="Email">
          <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
          <span>Email</span>
        </a>
      </div>

      <!-- Lado derecho: Copyright fino -->
      <div class="copyright">
        © {{ new Date().getFullYear() }} <span class="accent">—</span> GaloProano-Dev
      </div>

    </div>
  </footer>
</template>

<style scoped>
.footer {
  padding: 1.5rem 1.5rem; /* Se reduce drásticamente el alto */
  background-color: #030407;
  border-top: 1px solid rgba(168, 85, 247, 0.15);
}

.footer-container {
  max-width: 1100px;
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
  flex-wrap: wrap; /* Mantiene la flexibilidad en móviles */
}

/* Marca / Titular fino */
.footer-brand h3 {
  font-size: 1.1rem;
  font-weight: 700;
  color: #f8fafc;
  margin: 0;
}

.footer-brand p {
  font-size: 0.825rem;
  color: #94a3b8;
  margin: 0.2rem 0 0 0;
}

/* Redes sociales estilizadas */
.social-links {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.social-links a {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  color: #cbd5e1;
  text-decoration: none;
  font-size: 0.85rem;
  font-weight: 500;
  padding: 0.4rem 0.8rem;
  border-radius: 0.375rem;
  border: 1px solid rgba(255, 255, 255, 0.05);
  background-color: rgba(255, 255, 255, 0.02);
  transition: all 0.2s ease;
}

.icon {
  width: 14px;
  height: 14px;
  stroke: #c084fc;
}

.social-links a:hover {
  background-color: rgba(168, 85, 247, 0.1);
  border-color: rgba(168, 85, 247, 0.4);
  color: #ffffff;
  transform: translateY(-1px);
}

.social-links a:hover .icon {
  stroke: #e9d5ff;
  filter: drop-shadow(0 0 4px #a855f7);
}

/* Copyright discreto */
.copyright {
  font-size: 0.8rem;
  color: #64748b;
  white-space: nowrap;
}

.accent {
  color: #a855f7;
}

/* Adaptación responsive para pantallas pequeñas */
@media (max-width: 768px) {
  .footer-container {
    flex-direction: column;
    text-align: center;
    gap: 1.2rem;
  }

  .social-links {
    justify-content: center;
  }
}
</style>