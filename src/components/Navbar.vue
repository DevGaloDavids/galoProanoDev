<script setup>
import { ref } from 'vue'

const isMenuOpen = ref(false)

const navLinks = [
  { name: 'Sobre Mí', href: '#sobre-mi' },
  { name: 'Experiencia', href: '#experiencia' },
  { name: 'Formación', href: '#formacion' },
  { name: 'Contacto', href: '#contacto' }
]

function toggleMenu() { isMenuOpen.value = !isMenuOpen.value }
function closeMenu() { isMenuOpen.value = false }
</script>

<template>
  <header class="navbar-header">
    <nav class="navbar-container">
      <a href="#sobre-mi" class="logo">&lt;GaloProano-dev<span class="dot"> /&gt;</span></a>

      <button class="mobile-toggle" @click="toggleMenu" aria-label="Abrir menú">
        <span :class="{ 'bar-open': isMenuOpen }"></span>
        <span :class="{ 'bar-open': isMenuOpen }"></span>
        <span :class="{ 'bar-open': isMenuOpen }"></span>
      </button>

      <ul class="nav-links" :class="{ 'nav-active': isMenuOpen }">
        <li v-for="link in navLinks" :key="link.name">
          <a :href="link.href" @click="closeMenu">{{ link.name }}</a>
        </li>
      </ul>
    </nav>
  </header>
</template>

<style scoped>
.navbar-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: rgba(3, 5, 8, 0.85); /* Coincide con el nuevo fondo */
  backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(168, 85, 247, 0.15); /* Borde violeta muy suave */
  z-index: 1000;
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
}

.logo {
  font-size: 1.25rem;
  font-weight: 700;
  color: #f8fafc;
  text-decoration: none;
  font-family: monospace;
}

.dot {
  color: #a855f7;
}

.nav-links {
  display: flex;
  gap: 2rem;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links a {
  color: #94a3b8;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.nav-links a:hover {
  color: #c084fc;
}

.mobile-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
}

.mobile-toggle span {
  width: 25px;
  height: 3px;
  background-color: #f8fafc;
  border-radius: 2px;
}

@media (max-width: 768px) {
  .mobile-toggle { display: flex; }
  .nav-links {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #070a12;
    flex-direction: column;
    align-items: center;
    padding: 1.5rem 0;
    gap: 1.5rem;
    border-bottom: 1px solid rgba(168, 85, 247, 0.2);
    display: none;
  }
  .nav-links.nav-active { display: flex; }
}
</style>