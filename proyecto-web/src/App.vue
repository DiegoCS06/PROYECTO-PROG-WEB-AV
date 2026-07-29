<script setup>
import { ref, onMounted } from 'vue';
import AppHeader from './components/AppHeader.vue'
import HeroSection from './components/HeroSection.vue'
import NosotrosSection from './components/NosotrosSection.vue'
import ProcesoSection from './components/ProcesoSection.vue';
import ValoresSection from './components/ValoresSection.vue';
import TeamSection from './components/TeamSection.vue';
import ContactoSection from './components/ContactoSection.vue';
import AppFooter from './AppFooter.vue';
import CollectionModal from './components/CollectionModal.vue';
import RegisterModal from './components/RegisterModal.vue'
import LoginModal from './components/LoginModal.vue'

const data = ref([])
const error = ref('')

onMounted(async () => {
  try {
    const response = await fetch('/data/Catalogo.json')

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`)
    }
    data.value = await response.json()
    console.log(data.value)

  } catch (err) {
    error.value = 'No Products found! :('
    console.error(err)
  }

  const sesionGuardada = localStorage.getItem('ecoraSesion')

  if (sesionGuardada) {
  usuarioSesion.value = JSON.parse(sesionGuardada)
  }
})

const isModalOpen = ref(false)
const isRegisterOpen = ref(false)

const manejarRegistroExitoso = (nuevoUsuario) => {
  console.log('Usuario registrado:', nuevoUsuario)
}

const isLoginOpen = ref(false)
const usuarioSesion = ref(null)

const manejarInicioSesion = (usuario) => {
  usuarioSesion.value = usuario
  isLoginOpen.value = false
}

const cerrarSesion = () => {
  localStorage.removeItem('ecoraSesion')
  usuarioSesion.value = null
}
</script>

<template>
  <AppHeader :usuario="usuarioSesion"
  @open-register="isRegisterOpen = true"
  @open-login="isLoginOpen = true"
  @logout="cerrarSesion"
/>
  <HeroSection @open-collection="isModalOpen = true" />
  <NosotrosSection />
  <ValoresSection />
  <TeamSection />
  <ProcesoSection />
  <ContactoSection />
  <AppFooter />

  <CollectionModal v-if="isModalOpen" @close="isModalOpen = false" :catalogo="data.catalogo" />

  <RegisterModal
  v-if="isRegisterOpen"
  @close="isRegisterOpen = false"
  @registered="manejarRegistroExitoso"
  />
  <LoginModal
  v-if="isLoginOpen"
  @close="isLoginOpen = false"
  @login-success="manejarInicioSesion"
  />
</template>

<style>
/* Reset global y variables de Ecora */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:root {
  --ecora-cream: #F5F3EE;
  --ecora-stone: #716C59;
  --ecora-sand: #AEA897;
  --ecora-black: #0B0B09;
  --ecora-warm: #6F5F48;
  --ecora-gray: #A5A8AF;
  --ecora-error: #c0392b;
}

body {
  background: var(--ecora-cream);
  color: var(--ecora-black);
  font-family: 'Jost', sans-serif;
  -webkit-font-smoothing: antialiased;
}

img {
  max-width: 100%;
  display: block;
}

a {
  text-decoration: none;
  color: inherit;
}
</style>