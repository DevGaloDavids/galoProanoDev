<script setup>
import { onMounted, onUnmounted } from 'vue'
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import Experience from './components/Experience.vue'
import Education from './components/Education.vue'
import Footer from './components/Footer.vue'

let observer = null

onMounted(() => {
  const options = {
    root: null,
    // Define el margen en el centro de la pantalla para activar/desactivar la sección
    rootMargin: '-25% 0px -25% 0px',
    threshold: 0.15
  }

  observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-active')
      } else {
        entry.target.classList.remove('is-active')
      }
    })
  }, options)

  // Seleccionamos los contenedores de las secciones
  const sections = document.querySelectorAll('.scroll-section')
  sections.forEach((section) => observer.observe(section))
})

onUnmounted(() => {
  if (observer) observer.disconnect()
})
</script>

<template>
  <div class="app-layout">
    <Navbar />
    <main>
      <!-- La primera sección nace activa para que el Hero se vea nada más cargar -->
      <section class="scroll-section is-active">
        <HeroSection />
      </section>

      <section class="scroll-section">
        <Experience />
      </section>

      <section class="scroll-section">
        <Education />
      </section>
    </main>
    <Footer />
  </div>
</template>

<style scoped>
.app-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}

/* --- EFECTO ENFOQUE Y DIFUMINADO DINÁMICO --- */
.scroll-section {
  opacity: 0.2;
  filter: blur(6px);
  transform: translateY(20px) scale(0.97);
  transition: 
    opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1),
    filter 0.8s cubic-bezier(0.16, 1, 0.3, 1),
    transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
  will-change: opacity, filter, transform;
}

.scroll-section.is-active {
  opacity: 1;
  filter: blur(0px);
  transform: translateY(0) scale(1);
}
</style>