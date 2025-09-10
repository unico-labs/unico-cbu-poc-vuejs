<script setup>
import { ref, computed, watch, nextTick } from 'vue'
import { ByUnicoSDK } from 'idpay-b2b-sdk'
import logoHome from '../assets/logo-home.svg'

const emit = defineEmits(['back'])

const showModal = ref(false)
const error = ref('')
const transactionId = ref('')
const token = ref('')
const iframeKey = ref(0)
const sdkInitialized = ref(false)

const unicoIframeRef = ref(null)

const closeByUnicoSDKSession = () => {
  // Only try to close if SDK was initialized
  if (!sdkInitialized.value) {
    console.log('*** SDK not initialized yet, skipping close ***')
    return
  }

  console.log('*** EXECUTING closeByUnicoSDKSession ***')
  try {
    if (
      typeof ByUnicoSDK !== 'undefined' &&
      ByUnicoSDK &&
      typeof ByUnicoSDK.close === 'function'
    ) {
      console.log('*** CALLING ByUnicoSDK.close() ***')
      ByUnicoSDK.close()
      console.log('ByUnicoSDK session closed successfully.')
      sdkInitialized.value = false // Reset state
    } else {
      console.warn(
        'ByUnicoSDK.close method not fully available for calling, skipping close.'
      )
    }
  } catch (e) {
    console.error(
      'Error closing ByUnicoSDK session (likely ByUnicoSDK not fully initialized or active):',
      e
    )
  }
}

const handleCloseModal = () => {
  console.log('*** CLOSING MODAL - starting cleanup process ***')
  showModal.value = false
  closeByUnicoSDKSession()
  iframeKey.value += 1
}

const onFinishSdk = (result) => {
  console.log('Finish SDK:', result)
  console.log('*** SDK FINISHED - closing modal automatically ***')
  handleCloseModal()
}

const initializeAndOpenByUnicoSDK = async () => {
  if (!unicoIframeRef.value) {
    console.warn(
      'initializeAndOpenByUnicoSDK called but unicoIframeRef.value is null.'
    )
    return
  }

  const startInitialization = () => {
    console.log('*** STARTING ByUnicoSDK... ***')
    ByUnicoSDK.init({
      type: 'IFRAME',
      env: 'uat',
      token: token.value,
      element: unicoIframeRef.value,
    })

    // Mark as initialized
    sdkInitialized.value = true

    // Delay ByUnicoSDK.open call for 0.5 seconds (500ms)
    setTimeout(() => {
      console.log('*** OPENING ByUnicoSDK after 0.5s delay... ***')
      ByUnicoSDK.open({
        transactionId: transactionId.value,
        token: token.value,
        onFinish: onFinishSdk,
      })
    }, 500)
  }

  // Only close previous session if it was already initialized
  if (sdkInitialized.value) {
    console.log('*** SDK already initialized, closing previous session ***')
    closeByUnicoSDKSession()
    // Wait a bit before reinitializing
    setTimeout(startInitialization, 200)
  } else {
    // First initialization
    startInitialization()
  }
}

const handleOpenModal = () => {
  error.value = ''

  if (!token.value.trim()) {
    error.value = 'Error: Token is required'
    return
  }
  if (!transactionId.value.trim()) {
    error.value = 'Error: Transaction ID is required'
    return
  }

  showModal.value = true
}

// Watch for modal visibility and iframe ref availability
watch([showModal, unicoIframeRef], async ([isVisible, iframeRef]) => {
  if (isVisible && iframeRef) {
    console.log(
      'Modal is visible and iframe ref is ready. Attempting to initialize and open SDK.'
    )
    await nextTick() // Wait for DOM update
    initializeAndOpenByUnicoSDK()
  } else if (!isVisible && iframeRef) {
    console.log('Modal closed, iframe ref might still exist briefly.')
  }
})

// Usando v-model para inputs, mas mantendo controle do error
const clearError = () => {
  error.value = ''
}

const handleBack = () => {
  console.log('*** BACK BUTTON CLICKED - ModalTest ***')
  emit('back')
}

const isTransactionIdError = computed(() => error.value.includes('Transaction ID'))
const isTokenError = computed(() => error.value.includes('Token'))
</script>

<template>
  <div class="test-screen">
    <div class="test-header">
      <button class="back-button" @click="handleBack">
        ← Back
      </button>
      <img :src="logoHome" alt="Unico Logo" class="test-logo" />
      <h1>Modal Test</h1>
      <p>SDK displayed in overlay modal on page</p>
    </div>

    <div class="test-controls">
      <div class="input-group">
        <label for="modalTransactionId">Transaction ID:</label>
        <input
          id="modalTransactionId"
          v-model="transactionId"
          @input="clearError"
          placeholder="Enter the transaction ID"
          type="text"
          :class="['test-input', { error: isTransactionIdError }]"
        />
      </div>

      <div class="input-group">
        <label for="modalToken">Token:</label>
        <input
          id="modalToken"
          v-model="token"
          @input="clearError"
          placeholder="Enter the token"
          type="text"
          :class="['test-input', { error: isTokenError }]"
        />
      </div>

      <div v-if="error" class="error-message">{{ error }}</div>

      <div class="button-group">
        <button
          class="action-button modal-open-button"
          @click="handleOpenModal"
          :disabled="!transactionId || !token"
        >
          Open SDK in Modal
        </button>
      </div>
    </div>

    <!-- Modal -->
    <div v-if="showModal" class="modal-overlay">
      <div class="modal-content">
        <button class="modal-close-button" @click="handleCloseModal">
          ×
        </button>
        <div class="modal-iframe-wrapper">
          <div
            id="unico_iframe_embedded"
            ref="unicoIframeRef"
            :key="iframeKey"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Estilos já definidos no style.css global */
</style>
