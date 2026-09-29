<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { 
  User, 
  Briefcase, 
  GraduationCap, 
  Mail, 
  Menu, 
  X
} from 'lucide-vue-next'

const isMenuOpen = ref(false)
const isScrolled = ref(false)

const navLinks = [
  { name: 'Sobre mí', href: '#sobre-mi', icon: User },
  { name: 'Experiencia', href: '#experiencia', icon: Briefcase },
  { name: 'Formación', href: '#formacion', icon: GraduationCap },
  { name: 'Contacto', href: '#contacto', icon: Mail }
]

function toggleMenu() { isMenuOpen.value = !isMenuOpen.value }
function closeMenu() { isMenuOpen.value = false }

const handleScroll = () => {
  isScrolled.value = window.scrollY > 20
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <header :class="['navbar-header', { 'is-scrolled': isScrolled }]">
    <nav class="navbar-container">
      <!-- Logo -->
      <a href="#sobre-mi" class="logo">
        <span class="logo-bracket">&lt;</span>GaloProano-dev<span class="logo-slash"> /&gt;</span>
      </a>

      <!-- Desktop Navigation -->
      <div class="desktop-nav">
        <ul class="nav-links-pill">
          <li v-for="link in navLinks" :key="link.name">
            <a :href="link.href" class="nav-item">
              <component :is="link.icon" :size="18" class="nav-icon" />
              <span>{{ link.name }}</span>
            </a>
          </li>
        </ul>
      </div>

      <!-- Mobile Bubble Navigation -->
      <div class="mobile-controls">
        <div class="bubble-nav-container" :class="{ 'is-active': isMenuOpen }">
          
          <!-- Botón disparador -->
          <button 
            class="bubble-toggle" 
            :class="{ 'is-active': isMenuOpen }"
            @click="toggleMenu" 
            aria-label="Menú"
          >
            <Menu v-if="!isMenuOpen" :size="24" />
            <X v-else :size="24" />
          </button>

          <!-- Botones desplegables con su texto a la izquierda -->
          <div class="bubble-actions">
            <a 
              v-for="(link, index) in navLinks" 
              :key="link.name" 
              :href="link.href" 
              class="bubble-item-wrapper"
              :style="{ transitionDelay: `${index * 60}ms` }"
              @click="closeMenu"
            >
              <!-- Texto a la izquierda de la burbuja -->
              <span class="bubble-label">{{ link.name }}</span>
              
              <!-- Icono de la burbuja -->
              <div class="bubble-icon-box">
                <component :is="link.icon" :size="20" />
              </div>
            </a>
          </div>

        </div>
      </div>
    </nav>

    <!-- Teleport manda el overlay directamente al body para difuminar TODA la pantalla -->
    <Teleport to="body">
      <transition name="fade">
        <div 
          v-if="isMenuOpen" 
          class="mobile-overlay-dim"
          :style="{
          position: 'fixed',
          top: 0,
          left: 0,
          width: '100vw',
          height: '100vh',
          backgroundColor: 'rgba(3, 5, 8, 0.75)',
          backdropFilter: 'blur(12px) brightness(0.6)',
          webkitBackdropFilter: 'blur(12px) brightness(0.6)',
          zIndex: 999
      }" 
          @click="closeMenu"></div>
      </transition>
    </Teleport>
  </header>
</template>

<style scoped>
.navbar-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 1.5rem 0;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1000;
}

.navbar-header.is-scrolled {
  padding: 0.75rem 0;
  background-color: rgba(3, 5, 8, 0.75);
  backdrop-filter: blur(16px);
  border-bottom: 1px solid rgba(168, 85, 247, 0.2);
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 1.5rem;
}

/* Logo Styles */
.logo {
  font-size: 1.25rem;
  font-weight: 700;
  color: #f8fafc;
  text-decoration: none;
  font-family: 'JetBrains Mono', monospace;
  transition: transform 0.3s ease;
}

.logo:hover {
  transform: scale(1.05);
}

.logo-bracket, .logo-slash {
  color: #a855f7;
  transition: color 0.3s ease;
}

.logo:hover .logo-bracket,
.logo:hover .logo-slash {
  color: #c084fc;
  text-shadow: 0 0 10px rgba(168, 85, 247, 0.5);
}

/* Desktop Pill Nav */
.desktop-nav {
  display: block;
}

.nav-links-pill {
  display: flex;
  gap: 0.5rem;
  list-style: none;
  margin: 0;
  padding: 0.5rem;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 100px;
  backdrop-filter: blur(8px);
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 1rem;
  color: #94a3b8;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.9rem;
  border-radius: 100px;
  transition: all 0.3s ease;
}

.nav-item:hover {
  color: #f8fafc;
  background: rgba(168, 85, 247, 0.15);
}

.nav-icon {
  opacity: 0.7;
  transition: transform 0.3s ease;
}

.nav-item:hover .nav-icon {
  opacity: 1;
  transform: translateY(-1px);
}

/* Mobile Controls & Bubble Stack */
.mobile-controls {
  display: none;
  position: relative;
  z-index: 1001;
}

.bubble-nav-container {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  position: relative;
}

.bubble-toggle {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: none;
  background: linear-gradient(135deg, #a855f7 0%, #7c3aed 100%);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  z-index: 1002;
}

.bubble-toggle.is-active {
  transform: rotate(90deg);
}

/* Acciones desplegables */
.bubble-actions {
  position: absolute;
  top: 58px;
  right: 0;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.75rem;
  pointer-events: none;
  opacity: 0;
  transform: translateY(-10px) scale(0.9);
  transition: all 0.35s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  z-index: 1002;
}

.bubble-nav-container.is-active .bubble-actions {
  pointer-events: auto;
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* Item completo (Texto + Burbuja) */
.bubble-item-wrapper {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-decoration: none;
  transition: all 0.3s ease;
}

/* Etiqueta de texto a la izquierda */
.bubble-label {
  background: rgba(15, 17, 26, 0.95);
  color: #f8fafc;
  padding: 0.4rem 0.8rem;
  border-radius: 8px;
  font-size: 0.85rem;
  font-weight: 500;
  border: 1px solid rgba(168, 85, 247, 0.3);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  white-space: nowrap;
  letter-spacing: 0.3px;
  transition: all 0.2s ease;
}

/* Círculo de la burbuja */
.bubble-icon-box {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: #0f111a;
  border: 1px solid rgba(168, 85, 247, 0.5);
  color: #c084fc;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.5);
  transition: all 0.2s ease;
  flex-shrink: 0;
}

/* Interacción al pulsar/hover */
.bubble-item-wrapper:active .bubble-icon-box {
  transform: scale(0.92);
  background: #a855f7;
  color: white;
}

.bubble-item-wrapper:active .bubble-label {
  border-color: #a855f7;
  color: #c084fc;
}

/* Transitions para la animación de entrada */
.fade-enter-active, .fade-leave-active { 
  transition: opacity 0.35s ease, backdrop-filter 0.35s ease; 
}
.fade-enter-from, .fade-leave-to { 
  opacity: 0; 
}

@media (max-width: 768px) {
  .desktop-nav { display: none; }
  .mobile-controls { display: block; }
  .navbar-header { padding: 0.75rem 0; }
}
</style>

<!-- Estilos globales para el overlay renderizado fuera del scoped via Teleport -->
<style>
.mobile-overlay-dim {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(3, 5, 8, 0.65);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  z-index: 999;
}
</style>