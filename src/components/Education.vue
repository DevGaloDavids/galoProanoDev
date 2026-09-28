<template>
  <section id="formacion" class="section">
    <div class="container">
      <h2 class="section-title">Formación Académica & Certificaciones</h2>

      <div class="education-grid">
        <!-- Tarjeta 1: Educación Académica -->
        <div class="education-card">
          <div class="card-header">
            <div class="icon-box">
              <svg xmlns="http://www.w3.org/2000/svg" class="card-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M22 10v6M2 10l10-5 10 5-10 5z"/>
                <path d="M6 12v5c3 3 9 3 12 0v-5"/>
              </svg>
            </div>
            <div>
              <h3>Desarrollo de Aplicaciones Multiplataforma</h3>
              <span class="institution">Universidad Francisco de Vitoria</span>
            </div>
          </div>
          
          <div class="card-body">
            <span class="badge type-badge">Ciclo Formativo de Grado Superior</span>
            <span class="period-tag">2016 – 2018</span>
            <p class="description">
              Formación reglada enfocada al diseño de aplicaciones, bases de datos relacionales, estructuras de datos y principios del desarrollo de software.
            </p>
          </div>
        </div>

        <!-- Tarjeta 2: Licencias y Certificaciones -->
        <div class="education-card">
          <div class="card-header">
            <div class="icon-box">
              <svg xmlns="http://www.w3.org/2000/svg" class="card-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M12 15l-2 5l9-9l-9-9l2 5l-7 4l7 4z"/>
                <circle cx="12" cy="12" r="9"/>
              </svg>
            </div>
            <div>
              <h3>Test Unitario con JUnit y Mockito</h3>
              <span class="institution">Udemy</span>
            </div>
          </div>

          <div class="card-body">
            <span class="badge cert-badge">Certificación Profesional</span>
            <span class="period-tag">Expedición: abr. 2021</span>
            
            <p class="credential-id">
              <span class="label">ID Credencial:</span> UC-8e442da4-09d3-44c2-a4a7-f9f86df07358
            </p>

            <a 
              href="https://www.udemy.com/certificate/UC-8e442da4-09d3-44c2-a4a7-f9f86df07358/" 
              target="_blank" 
              rel="noopener noreferrer" 
              class="verify-btn"
            >
              <span>Mostrar credencial</span>
              <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path>
                <polyline points="15 3 21 3 21 9"></polyline>
                <line x1="10" y1="14" x2="21" y2="3"></line>
              </svg>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.section {
  padding: 4rem 1.5rem;
}

.container {
  max-width: 1050px;
  margin: 0 auto;
}

.section-title {
  font-size: 2rem;
  color: #f8fafc;
  margin-bottom: 2.5rem;
  border-left: 4px solid #1d4ed8; /* Azul corporativo GFT */
  padding-left: 1rem;
}

/* Grid de Tarjetas */
.education-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 1.5rem;
}

/* Tarjeta Estilo GFT / Dark Mode */
.education-card {
  background-color: #080c14;
  border: 1px solid rgba(30, 58, 138, 0.4);
  border-radius: 1rem;
  padding: 1.8rem 2rem;
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.8), 0 0 15px rgba(30, 58, 138, 0.1);
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.education-card:hover {
  border-color: rgba(59, 130, 246, 0.6);
  transform: translateY(-4px);
  box-shadow: 0 14px 35px -10px rgba(0, 0, 0, 0.9), 0 0 20px rgba(59, 130, 246, 0.2);
}

.card-header {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1.2rem;
}

.icon-box {
  width: 42px;
  height: 42px;
  border-radius: 0.75rem;
  background: rgba(30, 58, 138, 0.3);
  border: 1px solid rgba(59, 130, 246, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.card-icon {
  width: 22px;
  height: 22px;
  stroke: #60a5fa;
}

h3 {
  margin: 0 0 0.3rem 0;
  font-size: 1.15rem;
  color: #ffffff;
  line-height: 1.3;
}

.institution {
  color: #60a5fa;
  font-size: 0.9rem;
  font-weight: 500;
  display: block;
}

.card-body {
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
}

/* Badges y Fechas */
.badge {
  display: inline-block;
  align-self: flex-start;
  padding: 0.25rem 0.65rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 500;
}

.type-badge {
  background: rgba(30, 58, 138, 0.25);
  color: #93c5fd;
  border: 1px solid rgba(59, 130, 246, 0.25);
}

.cert-badge {
  background: rgba(16, 185, 129, 0.1);
  color: #34d399;
  border: 1px solid rgba(16, 185, 129, 0.25);
}

.period-tag {
  color: #64748b;
  font-size: 0.8rem;
  font-family: monospace;
}

.description {
  color: #94a3b8;
  font-size: 0.9rem;
  line-height: 1.5;
  margin: 0;
}

.credential-id {
  font-size: 0.78rem;
  color: #64748b;
  font-family: monospace;
  word-break: break-all;
  margin: 0;
}

.credential-id .label {
  color: #94a3b8;
}

/* Botón Verificar */
.verify-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  align-self: flex-start;
  margin-top: 0.5rem;
  padding: 0.4rem 0.9rem;
  border-radius: 9999px;
  border: 1px solid rgba(59, 130, 246, 0.4);
  background: transparent;
  color: #93c5fd;
  font-size: 0.8rem;
  text-decoration: none;
  transition: all 0.2s ease;
}

.verify-btn:hover {
  background: rgba(30, 58, 138, 0.4);
  border-color: #3b82f6;
  color: #ffffff;
}
</style>