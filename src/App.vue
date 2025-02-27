<script setup>
import { ref } from 'vue'
import { UnicoSDK } from 'idpay-b2b-sdk/index.js'

const iframeVisivel = ref(false)
const iniciarIframe = () => {
  iframeVisivel.value = true
}

const token = ''; // Inserir o token do processo
const processId = ''; // inserir o Id do processo

const Init = () => {
  UnicoSDK.init({
  env: 'uat',// Só irá ser preenchido se for ambiente de testes.
  token,
});
}

const Open = () => {
  UnicoSDK.open({
  transactionId: processId,
  token: token,
  onFinish: onFinishCallback
});
}

const onFinishCallback = process => {
  console.log('Process', process);
}

const Close = () => {
  UnicoSDK.close();
}

</script>
<template>
  <div class="container">
    <div> <img src="https://cdn.iconscout.com/icon/free/png-256/free-vuejs-1175052.png?f=webp" alt="Vue" class="logo"></div>
    <h1>Unico POC CBU Vue</h1>
    <div class="botoes">
      <button @click="Init">Init</button>
      <button @click="Open">Open</button>
      <button @click="Close">Close</button>
    </div>

  <div id="master-div" class="iframe-container">
  <div id="unico_iframe_embedded"></div>
  </div>

  </div>
</template>
<style scoped>

.container {
  text-align: center;
  padding-top: 40px;
  max-width: 1500px; 
  margin: 0 auto; 
  padding: 0 20px; 
  position: relative;
}

.iframe-container {
  width: 100%;
  max-width: none; 
  height: 50vh; 
  min-height: 400px;
  background-color: #e0e0e0; 
  border-radius: 12px; 
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); 
  display: flex; 
  align-items: center; 
  justify-content: center; 
  margin: 30px auto; 
  position: relative;
}


.logo {
  height: 100px;
  max-width: 100%;
}


.botoes {
  margin-top: 20px;
  display: flex; 
  flex-wrap: wrap; 
  justify-content: center; 
  gap: 15px; 
}

button {
  padding: 12px 24px; 
  font-size: 18px;
  cursor: pointer;
  border-radius: 8px;
  border: none; 
  background-color: #42b883;
  color: white; 
  transition: background-color 0.3s ease; 
}


button:hover {
  background-color: #35495e;
}

@media (max-width: 768px) {
  .iframe-container {
    width: 100%;
    left: 0;
    height: 50vh; 
    min-height: 300px; 
  }

  button {
    padding: 10px 16px; 
    font-size: 16px; 
  }
}

@media (max-width: 480px) {
  .iframe-container {
    height: 40vh; 
    min-height: 250px;
  }

  .logo {
    height: 110px; 
  }

  button {
    width: 100%; 
    padding: 12px; 
  }
}

</style>