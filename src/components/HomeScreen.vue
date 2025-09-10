<script setup>
import logoHome from '../assets/logo-home.svg'

const emit = defineEmits(['mode-select'])

const handleModeSelect = (mode) => {
  if (mode === 'modal') {
    console.log('*** MODAL BUTTON CLICKED ***')
  } else if (mode === 'fullscreen') {
    console.log('*** FULLSCREEN BUTTON CLICKED ***')
  } else if (mode === 'iframe') {
    console.log('*** IFRAME BUTTON CLICKED ***')
  }
  emit('mode-select', mode)
}
</script>

<template>
  <div class="home-screen">
    <div class="home-header">
      <img :src="logoHome" alt="Unico Logo" class="unico-logo" />
      <h1 class="home-title">Unico PoC By Unico - Vue</h1>
      <p class="home-subtitle">
        Choose the display mode to test the Unico SDK integration
      </p>
    </div>

    <div class="mode-selection">
      <button
        class="mode-button modal-button"
        @click="handleModeSelect('modal')"
      >
        <div class="button-icon">🪟</div>
        <h3>Modal Test</h3>
        <p>SDK displayed in overlay modal on page</p>
      </button>

      <button
        class="mode-button fullscreen-button"
        @click="handleModeSelect('fullscreen')"
      >
        <div class="button-icon">📺</div>
        <h3>Fullscreen Test</h3>
        <p>SDK taking up the entire browser screen</p>
      </button>

      <button
        class="mode-button iframe-button"
        @click="handleModeSelect('iframe')"
      >
        <div class="button-icon">🖼️</div>
        <h3>Iframe Box Test</h3>
        <p>SDK embedded in a box on the page</p>
      </button>
    </div>

    <footer class="home-footer">
      <div class="footer-content">
        <p>
          For detailed implementation guide, visit our
          <a 
            href="https://devcenter.unico.io/idcloud/integracao/integracao-by-unico/controlando-a-experiencia/sdk#como-comecar"
            target="_blank"
            rel="noopener noreferrer"
            class="documentation-link"
          >
            Official Documentation
          </a>
        </p>
      </div>
    </footer>
  </div>
</template>

<style>
/* Estilos já definidos no style.css global */
</style>
