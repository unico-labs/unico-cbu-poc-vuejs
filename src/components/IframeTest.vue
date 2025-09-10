<script setup>
import { ref } from 'vue'
import { ByUnicoSDK } from 'idpay-b2b-sdk'
import logoHome from '../assets/logo-home.svg'

const emit = defineEmits(['back'])

const transactionId = ref('')
const token = ref('')

// Removido pois agora usamos v-model

const initIframe = () => {
  ByUnicoSDK.init({
    env: 'uat',
    token: token.value,
  })
}

const onFinish = (transaction) => {
  console.log('>>>>>> onFinish', transaction)
}

const open = () => {
  ByUnicoSDK.open({
    transactionId: transactionId.value,
    token: token.value,
    onFinish,
  })
}

const close = () => {
  ByUnicoSDK.close()
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
      <h1>Iframe Box Test</h1>
      <p>SDK displayed in embedded iframe on the page</p>
    </div>

    <div class="test-controls">
      <div class="input-group">
        <label for="transactionId">Transaction ID:</label>
        <input
          id="transactionId"
          v-model="transactionId"
          placeholder="Enter the transaction ID"
          type="text"
          class="test-input"
        />
      </div>

      <div class="input-group">
        <label for="token">Token:</label>
        <input
          id="token"
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
          class="action-button open-button" 
          @click="open"
          :disabled="!transactionId || !token"
        >
          Open Flow
        </button>
        <button 
          data-testid="close" 
          class="action-button close-button" 
          @click="close"
        >
          Close
        </button>
      </div>
    </div>

    <div class="iframe-container">
      <div id="unico_iframe" class="iframe">
        <div id="unico_iframe_embedded"></div>
      </div>
    </div>
  </div>
</template>

<style>
/* Estilos já definidos no style.css global */
</style>
