<script setup>
import { ref } from 'vue'
import HomeScreen from './components/HomeScreen.vue'
import IframeTest from './components/IframeTest.vue'
import ModalTest from './components/ModalTest.vue'
import FullscreenTest from './components/FullscreenTest.vue'

const currentScreen = ref('home')

const handleModeSelect = (mode) => {
  console.log(`*** MODE SELECTED: ${mode} ***`)
  currentScreen.value = mode
}

const handleBackToHome = () => {
  console.log('*** BACK TO HOME CALLED ***')
  currentScreen.value = 'home'
}
</script>

<template>
  <div class="app-container">
    <HomeScreen v-if="currentScreen === 'home'" @mode-select="handleModeSelect" />
    <IframeTest v-else-if="currentScreen === 'iframe'" @back="handleBackToHome" />
    <ModalTest v-else-if="currentScreen === 'modal'" @back="handleBackToHome" />
    <FullscreenTest v-else-if="currentScreen === 'fullscreen'" @back="handleBackToHome" />
    <HomeScreen v-else @mode-select="handleModeSelect" />
  </div>
</template>

<style>
/* Estilos globais já definidos no style.css */
</style>
