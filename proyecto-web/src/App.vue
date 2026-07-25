<script setup>
import AppHeader from './components/AppHeader.vue'
import HeroSection from './components/HeroSection.vue'
import NosotrosSection from './components/NosotrosSection.vue'
import ProcesoSection from './components/ProcesoSection.vue';
import ValoresSection from './components/ValoresSection.vue';
import TeamSection from './components/TeamSection.vue';
import ContactoSection from './components/ContactoSection.vue';
import AppFooter from './AppFooter.vue';
</script>

<template>
  <AppHeader />
  <HeroSection/>
  <NosotrosSection />
  <ValoresSection/>
  <TeamSection/>
  <ProcesoSection/>
  <ContactoSection/>
  <AppFooter/>
</template>

<style>
/* Reset global y variables de Ecora */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:root {
  --ecora-cream:  #F5F3EE;
  --ecora-stone:  #716C59;
  --ecora-sand:   #AEA897;
  --ecora-black:  #0B0B09;
  --ecora-warm:   #6F5F48;
  --ecora-gray:   #A5A8AF;
  --ecora-error:  #c0392b;
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