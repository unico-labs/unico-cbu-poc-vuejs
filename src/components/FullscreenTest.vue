<script setup>
import { ref } from 'vue'
import { ByUnicoSDK } from 'idpay-b2b-sdk'
import logoHome from '../assets/logo-home.svg'

const emit = defineEmits(['back'])

const transactionId = ref('')
const token = ref('')
const isFullscreen = ref(false)

// Usando v-model para simplificar binding

const initIframe = () => {
  console.log('*** STARTING SDK in FULLSCREEN mode ***')
  ByUnicoSDK.init({
    env: 'uat',
    token: token.value,
  })
}

const onFinish = (transaction) => {
  console.log('>>>>>> onFinish Fullscreen', transaction)
  // Exit fullscreen mode when finished
  isFullscreen.value = false
}

const openFullscreen = () => {
  console.log('*** OPENING SDK in FULLSCREEN ***')
  
  // First activate fullscreen mode
  isFullscreen.value = true
  
  // Wait a bit for CSS to be applied before opening
  setTimeout(() => {
    ByUnicoSDK.open({
      transactionId: transactionId.value,
      token: token.value,
      onFinish,
    })
  }, 100)
}

const handleBack = () => {
  emit('back')
}
</script>

<template>
  <div class="test-screen">
    <div class="test-header">
      <button class="back-button" @click="handleBack">
        ← Back
      </button>
      <img :src="logoHome" alt="Unico Logo" class="test-logo" />
      <h1>Fullscreen Test</h1>
      <p>SDK taking up the entire browser screen</p>
      <p v-if="isFullscreen" class="fullscreen-hint">
        Fullscreen mode active - flow will close automatically when complete
      </p>
      <p v-else class="fullscreen-ready">
        Fill in the fields below and click "Open Fullscreen" to start
      </p>
    </div>

    <div :class="['test-controls', { 'hidden-in-fullscreen': isFullscreen }]">
      <div class="input-group">
        <label for="fullscreenTransactionId">Transaction ID:</label>
        <input
          id="fullscreenTransactionId"
          v-model="transactionId"
          placeholder="Enter the transaction ID"
          type="text"
          class="test-input"
        />
      </div>

      <div class="input-group">
        <label for="fullscreenToken">Token:</label>
        <input
          id="fullscreenToken"
          v-model="token"
          placeholder="Enter the token"
          type="text"
          class="test-input"
        />
      </div>

      <div class="button-group">
        <button
          data-testid="init"
          class="action-button init-button"
          @click="initIframe"
          :disabled="!token"
        >
          Initialize SDK
        </button>
        <button
          data-testid="open"
          class="action-button fullscreen-open-button"
          @click="openFullscreen"
          :disabled="!transactionId || !token"
        >
          Open Fullscreen
        </button>
      </div>

      <div v-if="!isFullscreen" class="fullscreen-instructions">
        <p><strong>Instructions:</strong> Enter your Token and Transaction ID above, then click "Open Fullscreen" to start the SDK in full screen mode.</p>
      </div>
    </div>

    <div class="fullscreen-container">
      <div 
        id="unico_iframe" 
        :class="['fullscreen-iframe', { fullscreen: isFullscreen }]"
      >
        <div id="unico_iframe_embedded"></div>
      </div>
    </div>
  </div>
</template>

<style>
/* Estilos já definidos no style.css global */
</style>
