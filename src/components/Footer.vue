<template>
  <footer id="contacto" class="footer">
    <div class="container">
      <h3>¿Hablamos?</h3>
      <p>Estoy abierto a nuevas oportunidades profesionales y colaboraciones.</p>
      
      <div class="social-links">
        <a href="https://www.linkedin.com/in/galodavid-proano/" target="_blank" rel="noopener">LinkedIn</a>
        <a href="https://github.com/DevGaloDavids" target="_blank" rel="noopener">GitHub</a>
        <a href="mailto:galodavid.proano@gmail.com">Email</a>
      </div>

      <p class="copyright">© {{ new Date().getFullYear() }} — GaloProano-Dev</p>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  padding: 4rem 1.5rem 2rem 1.5rem;
  background-color: #020305; /* El punto más oscuro de la página */
  border-top: 1px solid rgba(168, 85, 247, 0.15);
  text-align: center;
}

.container { max-width: 600px; margin: 0 auto; }
h3 { font-size: 1.8rem; color: #ffffff; margin-bottom: 0.5rem; }
p { color: #94a3b8; }

.social-links {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  margin: 2rem 0;
}

.social-links a {
  color: #c084fc;
  text-decoration: none;
  font-weight: 600;
  padding: 0.6rem 1.2rem;
  border: 1px solid rgba(168, 85, 247, 0.25);
  border-radius: 0.5rem;
  background-color: rgba(168, 85, 247, 0.03);
  transition: all 0.2s ease;
}

.social-links a:hover {
  background-color: #a855f7;
  color: #ffffff;
  box-shadow: 0 0 20px rgba(168, 85, 247, 0.4);
  transform: translateY(-2px);
}

.copyright { font-size: 0.85rem; color: #475569; margin-top: 2rem; }
</style>