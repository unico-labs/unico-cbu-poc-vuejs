<div align="center">
  <img src="./src/assets/logo-home.svg" alt="Unico Logo" width="200"/>
  <h1>CBU PoC - Vue.js Testing Application</h1>
  <p><em>Proof of Concept for testing Unico SDK integration in Vue.js with different display modes</em></p>
</div>

---

## 📋 Overview

This Vue.js application serves as a **Proof of Concept (PoC)** for testing **Unico SDK** integration. It demonstrates three different ways to implement and display the SDK flow:

- **🪟 Modal Test** - SDK displayed in overlay modal on page
- **📺 Fullscreen Test** - SDK taking up the entire browser screen
- **🖼️ Iframe Box Test** - SDK embedded in a box on the page

## 💻 Compatibility

### Compatible Devices

- You can check the devices tested in our laboratories in <a href='https://devcenter.unico.io/idcloud/integracao/integracao-by-unico/visao-geral#dispositivos-compativeis'>this</a> list of compatible devices.


## 🚀 Getting Started

### Prerequisites
- **Node.js** (version 16 or higher)
- **npm** 
- **Unico Service Account** with test environment access
- **Valid Token and Transaction ID** from Unico API

### Installation and Usage

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd unico-cbu-poc-vuejs
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```

4. **Open in browser:**
   Navigate to `http://localhost:4200`

### How to Use

1. **Select a test mode:** Choose from Modal, Fullscreen, or Iframe Box
2. **Enter your credentials:** Input your Token and Transaction ID  
3. **Initialize SDK:** Click "Initialize SDK" (if available)
4. **Start the flow:** Click the respective "Open" button to begin the Unico flow
5. **Complete the process:** Follow the Unico SDK flow to completion

### Project Structure
```
src/
├── components/
│   ├── HomeScreen.vue      # Main selection screen
│   ├── ModalTest.vue       # Modal implementation
│   ├── FullscreenTest.vue  # Fullscreen implementation  
│   └── IframeTest.vue      # Iframe implementation
├── assets/
│   └── logo-home.svg       # Official Unico logo
├── App.vue                 # Main app component with routing
├── style.css               # Global styles
└── main.js                 # App entry point
```


## ✨ Available Methods

**init(options)**

This method initializes the SDK, preloading assets to create the most fluid experience for the end user. At this point, you need to send the token received as a result of CreateProcess.

**Parameters:**

options - an object with the following configuration properties:

**type**

The type of flow that will be initialized. In by Unico we use the "IFRAME" option.

**token**

Receives the token from the created process. This token is important for authenticating the journey and ensuring that only authorized domains use it (can be obtained when creating the process via API).

```javascript
import { ByUnicoSDK } from "idpay-b2b-sdk";

ByUnicoSDK.init({
  env: 'uat', // Only filled if it's a test environment
  token,
});
```

---

**open(options)**

This method opens the by Unico experience. For IFRAME type flow, this function displays the pre-loaded iframe and starts the messaging flow between the client page and the by Unico experience.

**Parameters:**

**options** - an object with configuration properties:

**processId**

Receives the ID of the created process. This ID is important for obtaining process details and performing the entire flow correctly (can be obtained when creating the process via API).

**token**

Receives the token from the created process. This token is important for authenticating the journey and ensuring that only authorized domains use it (can be obtained when creating the process via API).

**onFinish(process)**

Receives a callback function that will be executed at the end of the by Unico journey, passing the process object as an argument with the following data: { captureConcluded, concluded, id }

```javascript
const processId = '9bc22bac-1e64-49a5-94d6-9e4f8ec9a1bf';
```

```javascript
const process = {
  id: '9bc22bac-1e64-49a5-94d6-9e4f8ec9a1bf',
  concluded: true,
  captureConcluded: true
};
```

```javascript
const onFinishCallback = process => {
  console.log('Process', process);
}
```

```javascript
ByUnicoSDK.open({
  transactionId: processId,
  token: token,
  onFinish: onFinishCallback
});
```

```javascript
ByUnicoSDK.close();
```

---

## 🔧 Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production  
- `npm run preview` - Preview production build

## 📚 Documentation

For detailed implementation guides and API reference, visit the official Unico documentation:

**📖 [Official Documentation](https://devcenter.unico.io/idcloud/integracao/integracao-by-unico/controlando-a-experiencia/sdk#como-comecar)**

## 📞 Support

For technical support and project integration assistance:
- Contact your Unico project manager
- Reach out to Unico support team
- Consult the [Developer Center](https://devcenter.unico.io/)

---

<div align="center">
  <p>Built with ❤️ for Unico clients</p>
  <p><em>© 2024 Unico. All rights reserved.</em></p>
</div>

