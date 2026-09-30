<template>
  <section class="experience-section" id="experiencia">
    <div class="container">
      <h2 class="section-title">Experiencia Profesional</h2>

      <!-- Timeline Scrollable -->
      <div ref="timelineWrapper" class="timeline-wrapper">
        <div class="timeline-container">
          <!-- Línea neón horizontal -->
          <div class="neon-line"></div>

          <div 
            v-for="(item, index) in experiences" 
            :key="index" 
            class="timeline-node"
            :class="{ active: selectedIndex === index }"
            @click="selectExperience(index)"
          >
            <!-- Empresa Arriba (Logo Prominente) -->
            <div class="node-top">
              <span class="company-name">{{ item.company }}</span>
              <div class="logo-box">
                <img :src="item.companyLogo" :alt="item.company" class="logo logo-blend" />
              </div>
            </div>

            <!-- Punto Selector Neón -->
            <div class="neon-dot">
              <div class="inner-dot"></div>
            </div>

            <!-- Cliente Abajo (Logo Prominente) -->
            <div class="node-bottom">
              <div class="logo-box" :class="{ 'client-logo-group': item.projects }">
                <template v-if="item.projects">
                  <img
                    v-for="project in item.projects"
                    :key="project.client"
                    :src="project.clientLogo"
                    :alt="project.client"
                    class="logo logo-blend"
                  />
                </template>
                <img v-else :src="item.clientLogo" :alt="item.client" class="logo logo-blend" />
              </div>
              <span class="client-name">
                {{ item.projects ? item.projects.map(project => project.client).join(' · ') : `Cliente: ${item.client}` }}
              </span>
              <span class="period-tag">{{ item.period }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Tarjeta Detalle de la Etapa Seleccionada -->
      <div class="detail-card-stage">
        <transition :name="`slide-${transitionDirection}`">
          <div
            class="detail-card"
            :key="selectedIndex"
            @touchstart.passive="handleTouchStart"
            @touchend.passive="handleTouchEnd"
            @touchcancel="clearTouchStart"
          >
            <div class="card-header">
              <div>
                <h3>{{ experiences[selectedIndex].role }}</h3>
                <p class="subtitle">
                  <span class="company-highlight">{{ experiences[selectedIndex].company }}</span> 
                  <template v-if="experiences[selectedIndex].projects">
                    <span> | {{ experiences[selectedIndex].projects.length }} proyectos</span>
                  </template>
                  <template v-else>
                    <span> | Cliente: </span>
                    <span class="client-highlight">{{ experiences[selectedIndex].client }}</span>
                  </template>
                </p>
              </div>
              <span class="duration-badge">{{ formatDuration(experiences[selectedIndex]) }}</span>
            </div>

            <template v-if="experiences[selectedIndex].projects">
              <section
                v-for="project in experiences[selectedIndex].projects"
                :key="project.client"
                class="project-detail"
              >
                <div class="project-header">
                  <div>
                    <h4>{{ project.client }}</h4>
                    <p class="project-role">{{ project.role }}</p>
                  </div>
                  <span class="project-period">{{ project.period }}</span>
                </div>
                <div class="description" v-html="project.description"></div>
                <div class="tech-stack">
                  <h4>Tecnologías utilizadas:</h4>
                  <div class="tags">
                    <span v-for="tech in project.technologies" :key="tech" class="tech-tag">
                      {{ tech }}
                    </span>
                  </div>
                </div>
              </section>
            </template>
            <template v-else>
              <div class="description" v-html="experiences[selectedIndex].description"></div>
              <div class="tech-stack">
                <h4>Tecnologías utilizadas:</h4>
                <div class="tags">
                  <span
                    v-for="(tech, tIndex) in experiences[selectedIndex].technologies"
                    :key="tIndex"
                    class="tech-tag"
                  >
                    {{ tech }}
                  </span>
                </div>
              </div>
            </template>
          </div>
        </transition>
      </div>
    </div>
  </section>
</template>

<script setup>
import { nextTick, ref } from 'vue'

const selectedIndex = ref(0)
const transitionDirection = ref('next')
const touchStart = ref(null)
const timelineWrapper = ref(null)
const timelineAnimationDuration = 450
let timelineAnimationFrame = 0

function centerTimelineNode(index) {
  const wrapper = timelineWrapper.value
  const node = wrapper?.querySelectorAll('.timeline-node')[index]
  if (!wrapper || !node) return

  const wrapperRect = wrapper.getBoundingClientRect()
  const nodeRect = node.getBoundingClientRect()
  const targetScroll = wrapper.scrollLeft + nodeRect.left - wrapperRect.left + nodeRect.width / 2 - wrapper.clientWidth / 2
  const maxScroll = wrapper.scrollWidth - wrapper.clientWidth
  const startScroll = wrapper.scrollLeft
  const endScroll = Math.max(0, Math.min(targetScroll, maxScroll))
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches

  cancelAnimationFrame(timelineAnimationFrame)

  if (reduceMotion) {
    wrapper.scrollLeft = endScroll
    return
  }

  const startTime = performance.now()
  const animateScroll = (currentTime) => {
    const progress = Math.min((currentTime - startTime) / timelineAnimationDuration, 1)
    const easedProgress = progress * progress * (3 - 2 * progress)

    wrapper.scrollLeft = startScroll + (endScroll - startScroll) * easedProgress

    if (progress < 1) {
      timelineAnimationFrame = requestAnimationFrame(animateScroll)
    } else {
      timelineAnimationFrame = 0
    }
  }

  timelineAnimationFrame = requestAnimationFrame(animateScroll)
}

function selectExperience(index) {
  if (index < 0 || index >= experiences.value.length) return

  if (index === selectedIndex.value) {
    nextTick(() => centerTimelineNode(index))
    return
  }

  transitionDirection.value = index > selectedIndex.value ? 'next' : 'previous'
  selectedIndex.value = index
  nextTick(() => centerTimelineNode(index))
}

function handleTouchStart(event) {
  if (event.touches.length !== 1) {
    touchStart.value = null
    return
  }

  const touch = event.touches[0]
  touchStart.value = { x: touch.clientX, y: touch.clientY }
}

function handleTouchEnd(event) {
  if (!touchStart.value || event.changedTouches.length !== 1) {
    touchStart.value = null
    return
  }

  const touch = event.changedTouches[0]
  const deltaX = touch.clientX - touchStart.value.x
  const deltaY = touch.clientY - touchStart.value.y
  touchStart.value = null

  if (Math.abs(deltaX) < 50 || Math.abs(deltaX) < Math.abs(deltaY) * 1.2) return

  selectExperience(selectedIndex.value + (deltaX < 0 ? 1 : -1))
}

function clearTouchStart() {
  touchStart.value = null
}

function formatDuration(experience) {
  const [startYear, startMonth] = experience.startDate.split('-').map(Number)
  const endDate = experience.endDate ? experience.endDate.split('-') : null
  const endYear = endDate ? Number(endDate[0]) : new Date().getFullYear()
  const endMonth = endDate ? Number(endDate[1]) : new Date().getMonth() + 1
  const totalMonths = (endYear - startYear) * 12 + endMonth - startMonth
  const years = Math.floor(totalMonths / 12)
  const months = totalMonths % 12
  const parts = []

  if (years > 0) parts.push(`${years} ${years === 1 ? 'año' : 'años'}`)
  if (months > 0) parts.push(`${months} ${months === 1 ? 'mes' : 'meses'}`)

  const duration = parts.length > 0 ? parts.join(' y ') : 'Menos de 1 mes'
  return experience.endDate ? duration : `${duration} · En curso`
}

const experiences = ref([
  {
    company: 'Worldline Global',
    companyLogo: '/logos/wl.png',
    client: 'Proyecto local',
    clientLogo: '/logos/wl.png',
    role: 'Back-End Developer',
    startDate: '2017-12',
    endDate: '2018-03',
    period: 'Dic. 2017 - Mar. 2018',
    description: `
      <p class="mb-3">Incorporación mediante programa intensivo de formación y prácticas profesionales orientado al desarrollo e integración de software para clientes de gran escala.</p>
      <ul class="list-disc pl-5 space-y-1 text-sm text-slate-300">
        <li>Formación técnica especializada:</strong> Capacitación intensiva (1 mes) en arquitectura y programación con Natural ADABAS.</li>
        <li>Desarrollo y soporte:</strong> Participación durante 3 meses en proyectos reales para diversos clientes, aplicando lógica de negocio, mantenimiento, optimización y resolución de incidencias en Natural ADABAS.</li>
        <li>Trabajo en equipo y adaptabilidad:</strong> Integración rápida en metodologías corporativas y colaboración con equipos multidisciplinares.</li>
      </ul>
    `,
    technologies: ['Natural ADABAS']
  },
  {
    company: 'NTT DATA',
    companyLogo: '/logos/ntt.webp',
    client: 'BBVA',
    clientLogo: '/logos/bbva.png',
    role: 'Back-End Developer',
    startDate: '2018-03',
    endDate: '2021-04',
    period: 'Mar. 2018 - Abr. 2021',
    description: `
      <p class="mb-3">Desarrollo de soluciones de software e integración de sistemas para el sector bancario (BBVA) a través de NTT Data. Participación en el ciclo de vida completo de proyectos estratégicos, desde el desarrollo de evolutivos en producción hasta la arquitectura y construcción backend desde cero.</p>
      <ul class="list-disc pl-5 space-y-1 text-sm text-slate-300">
        <li>Proyecto "Mis Conversaciones": Mantenimiento y Evolutivos: Análisis, diseño e implementación de nuevas funcionalidades y evolutivos directamente en entornos de producción.</li>
        <li>Proyecto "Mis Viajes": Desarrollo Backend End-to-End: Diseño y desarrollo integral de la arquitectura backend desde cero, incluyendo la lógica de negocio en librerías y el modelado de base de datos.</li>
        <li>Calidad de Código y CI/CD: Implementación de pruebas unitarias automatizadas garantizando la calidad de las entregas. Uso de herramientas de integración continua y control de versiones para despliegues fluidos.</li>
      </ul>
    `,
    technologies: ['Java', 'APX', 'JUnit', 'Mockito', 'Oracle', 'MongoDB', 'Jenkins', 'Bamboo', 'Git']
  },
  {
    company: 'Grupo GFT',
    companyLogo: '/logos/gft.png',
    role: 'Front-End & Full-stack Developer',
    startDate: '2021-04',
    endDate: '2024-01',
    period: 'Abr. 2021 - Ene. 2024',
    projects: [
      {
        client: 'Mapfre',
        clientLogo: '/logos/mapfre2.webp',
        role: 'Front-End Developer',
        period: 'Abr. 2021 - Sep. 2021',
        description: `
          <p class="mb-3">Consultoría y desarrollo de software para aseguradora a través de GFT. Participación en proyectos transversales el desarrollo frontend.</p>
          <ul class="list-disc pl-5 space-y-1 text-sm text-slate-300">
            <li>Desarrollo y mantenimiento de evolutivos sobre la plataforma de gestión interna de empleados/operadores.</li>
            <li>Modernización y modificación de interfaces de usuario utilizando AngularJS, mejorando la usabilidad y la eficiencia operativa en entorno de producción.</li>
          </ul>
        `,
        technologies: ['HTML/CSS', 'AngularJS', 'CSS', 'SQL']
      },
      {
        client: 'BBVA',
        clientLogo: '/logos/bbva.png',
        role: 'Full-stack Developer',
        period: 'Sep. 2021 - Ene. 2024',
        description: `
          <p class="mb-3">Consultoría y desarrollo de software bancario a través de GFT. Participación en proyectos transversales el desarrollo backend.</p>
          <ul class="list-disc pl-5 space-y-1 text-sm text-slate-300">
            <li>Análisis e implementación de evolutivos complejos en producción sobre la arquitectura técnica de la entidad.</li>
            <li>Desarrollo backend avanzado utilizando la arquitectura APX (Java) y la arquitectura previa LRBA.</li>
            <li>Optimización y modelado de consultas en base de datos Oracle.</li>
          </ul>
        `,
        technologies: ['Java', 'APX', 'LRBA']
      }
    ]
  },
  {
    company: 'Minsait',
    companyLogo: '/logos/minsait.png',
    client: 'BBVA',
    clientLogo: '/logos/bbva.png',
    role: 'Full-stack Developer',
    startDate: '2024-02',
    endDate: null,
    period: 'Feb. 2024 - Actualidad',
    description: `
      <p class="mb-3">Desarrollo e integración de soluciones sobre el CRM estratégico de BBVA a través de Indra. Especialización en arquitectura orientada a eventos, integraciones cloud y extensión de funcionalidades para la gestión de clientes a gran escala.</p>
      <ul class="list-disc pl-5 space-y-1 text-sm text-slate-300">
        <li>Desarrollo y mantenimiento de evolutivos en producción para el CRM de la entidad, implementando lógica asíncrona y reactiva basada en JavaScript.</li>
        <li>Diseño y desarrollo de Custom Activities mediante Node.js para la integración directa con Salesforce, permitiendo automatizaciones y flujos de trabajo avanzados en la plataforma. Gestión y despliegue de componentes en infraestructura cloud (Heroku, AWS).</li>
        <li>Uso continuo de pipelines de integración y despliegue continuo (Jenkins) y control de versiones (GitHub) para asegurar entregas ágiles y calidad de código en entorno crítico de producción.</li>
      </ul>
    `,
    technologies: ['Node.js', 'JavaScript ', 'Salesforce', 'AWS', 'Heroku', 'Jenkins', 'Git']
  }
])
</script>

<style scoped>
.experience-section {
  padding: 4rem 1.5rem;
  overflow: hidden;
}

.container {
  max-width: 1050px;
  margin: 0 auto;
}

.section-title {
  font-size: 2rem;
  color: #f8fafc;
  margin-bottom: 2.5rem;
  border-left: 4px solid #a855f7;
  padding-left: 1rem;
}

/* Contenedor Scrollable Horizontal */
.timeline-wrapper {
  width: 100%;
  overflow-x: auto;
  padding: 2.5rem max(2rem, calc((100% - 170px) / 2));
  margin-bottom: 2rem;
  scrollbar-width: thin;
  scrollbar-color: #a855f7 #080c14;
}

.timeline-wrapper::-webkit-scrollbar {
  height: 6px;
}
.timeline-wrapper::-webkit-scrollbar-thumb {
  background: #a855f7;
  border-radius: 4px;
}

.timeline-container {
  display: flex;
  justify-content: space-between;
  min-width: 950px;
  position: relative;
  align-items: center;
  padding: 0;
}

/* Línea Horizontal Neón Azul */
.neon-line {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 3px;
  background: #a855f7;
  box-shadow: 0 0 12px #a855f7, 0 0 20px #a855f7;
  transform: translateY(-50%);
  z-index: 1;
}

/* Nodos */
.timeline-node {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  z-index: 2;
  cursor: pointer;
  transition: transform 0.25s ease;
  width: 170px;
}

.timeline-node:hover {
  transform: scale(1.06);
}

.node-top, .node-bottom {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  height: 100px;
}

.node-top { justify-content: flex-end; }
.node-bottom { justify-content: flex-start; }

.company-name, .client-name {
  font-size: 0.85rem;
  color: #94a3b8;
  text-align: center;
  white-space: nowrap;
}

.client-name { font-size: 0.78rem; color: #64748b; }

.period-tag {
  font-size: 0.72rem;
  color: #38bdf8;
  font-family: monospace;
}

/* Contenedor y dimensiones ampliadas del logo */
.logo-box {
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.client-logo-group {
  gap: 1rem;
}

.client-logo-group .logo {
  max-width: 62px;
  max-height: 42px;
}

.logo {
  max-height: 52px;
  max-width: 130px;
  object-fit: contain;
  filter: grayscale(40%) brightness(0.95);
  transition: all 0.3s ease;
}

.logo-blend {
  mix-blend-mode: lighten;
}

/* Punto Neón */
.neon-dot {
  width: 22px;
  height: 22px;
  border-radius: 50%;
  background: #030508;
  border: 2px solid #0284c7;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 12px 0;
  transition: all 0.3s ease;
  box-shadow: 0 0 6px rgba(56, 189, 248, 0.3);
}

.inner-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: transparent;
  transition: background 0.3s ease;
}

/* Estado Activo */
.timeline-node.active .neon-dot {
  border-color: #38bdf8;
  box-shadow: 0 0 16px #38bdf8, 0 0 28px #0284c7;
  transform: scale(1.3);
}

.timeline-node.active .inner-dot {
  background: #38bdf8;
}

.timeline-node.active .logo {
  filter: grayscale(0%) brightness(1.2);
  transform: scale(1.1);
}

.timeline-node.active .company-name {
  color: #ffffff;
  font-weight: 700;
}

/* Tarjeta Detalle */
.detail-card-stage {
  display: grid;
}

.detail-card {
  grid-area: 1 / 1;
  min-width: 0;
  background-color: #080c14;
  border: 1px solid rgba(56, 189, 248, 0.25);
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.9), 0 0 15px rgba(56, 189, 248, 0.05);
  border-radius: 1rem;
  padding: 2rem;
  margin-top: 1rem;
  touch-action: pan-y;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
  flex-wrap: wrap;
  gap: 1rem;
}

.card-header h3 {
  margin: 0;
  font-size: 1.4rem;
  color: #ffffff;
}

.project-detail + .project-detail {
  margin-top: 1.5rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(148, 163, 184, 0.2);
}

.project-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 0.75rem;
}

.project-header h4 {
  margin: 0;
  color: #f8fafc;
  font-size: 1rem;
}

.project-role {
  margin: 0.25rem 0 0;
  color: #94a3b8;
  font-size: 0.875rem;
}

.project-period {
  flex-shrink: 0;
  color: #38bdf8;
  font-family: monospace;
  font-size: 0.8rem;
}

.subtitle {
  margin: 0.3rem 0 0 0;
  color: #64748b;
  font-size: 0.95rem;
}

.company-highlight { color: #38bdf8; font-weight: 600; }
.client-highlight { color: #c084fc; font-weight: 600; }

.duration-badge {
  background: rgba(56, 189, 248, 0.1);
  color: #38bdf8;
  border: 1px solid rgba(56, 189, 248, 0.3);
  padding: 0.3rem 0.8rem;
  border-radius: 9999px;
  font-size: 0.8rem;
  font-family: monospace;
}

.description {
  color: #cbd5e1;
  line-height: 1.6;
  margin-bottom: 1.5rem;
}

.tech-stack h4 {
  font-size: 0.9rem;
  color: #94a3b8;
  margin-bottom: 0.6rem;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.tech-tag {
  background: rgba(255, 255, 255, 0.04);
  color: #e2e8f0;
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 0.3rem 0.7rem;
  border-radius: 0.3rem;
  font-size: 0.8rem;
  font-family: monospace;
}

.slide-next-enter-active,
.slide-next-leave-active,
.slide-previous-enter-active,
.slide-previous-leave-active {
  transition:
    opacity 450ms cubic-bezier(0.33, 0, 0.67, 1),
    transform 450ms cubic-bezier(0.33, 0, 0.67, 1);
}

.slide-next-enter-from {
  opacity: 0;
  transform: translateX(48px);
}

.slide-next-leave-to {
  opacity: 0;
  transform: translateX(-48px);
}

.slide-previous-enter-from {
  opacity: 0;
  transform: translateX(-48px);
}

.slide-previous-leave-to {
  opacity: 0;
  transform: translateX(48px);
}

@media (prefers-reduced-motion: reduce) {
  .slide-next-enter-active,
  .slide-next-leave-active,
  .slide-previous-enter-active,
  .slide-previous-leave-active {
    transition-duration: 0.01ms;
  }
}
</style>