<template>
  <section id="sobre-mi" class="section hero">
    <div class="container">
      <span class="badge">Bienvenido a mi perfil</span>
      <h1>Soy <span class="highlight">Galo David Proaño</span></h1>
      <h2>Full-Stack software developer</h2>
      <p>
        Senior Software Engineer especializado en la creación e integración de soluciones escalables para el sector financiero y asegurador. 
        Con una evolución constante desde las bases del desarrollo Backend en Java/APX y Frontend con AngularJS, actualmente me enfoco en arquitecturas orientadas a eventos e integraciones Cloud. 
        Apasionado por resolver retos técnicos complejos en entornos críticos de producción.
      </p>
    </div>
  </section>
</template>

<style scoped>
.hero {
  min-height: 100dvh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 100px;
  position: relative;
  overflow: hidden;
}

/* Glow morado más tenue y ambiental */
.glow-effect {
  position: absolute;
  top: 15%;
  left: 50%;
  transform: translateX(-50%);
  width: 400px;
  height: 400px;
  background: rgba(168, 85, 247, 0.08); /* Morado sutil */
  border-radius: 50%;
  filter: blur(120px);
  pointer-events: none;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
  padding: 0 1.5rem;
  position: relative;
  z-index: 1;
}

.badge {
  background-color: rgba(168, 85, 247, 0.08);
  color: #c084fc;
  padding: 0.5rem 1.2rem;
  border-radius: 9999px;
  font-size: 0.875rem;
  font-weight: 600;
  border: 1px solid rgba(168, 85, 247, 0.25);
  font-family: monospace;
}

h1 {
  font-size: clamp(2.5rem, 5vw, 4rem);
  margin: 1.5rem 0 0.5rem 0;
  color: #ffffff;
}

.highlight {
  color: #a855f7;
  text-shadow: 0 0 25px rgba(168, 85, 247, 0.3);
}

h2 {
  font-size: clamp(1.2rem, 3vw, 1.8rem);
  color: #94a3b8;
  margin-bottom: 1.5rem;
  font-weight: 400;
}

p {
  color: #cbd5e1;
  font-size: 1.1rem;
  line-height: 1.7;
}
</style>