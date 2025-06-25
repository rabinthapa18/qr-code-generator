<script setup lang="ts">
import TextBox from "./components/TextBox.vue";
import QRCodeDisplay from "./components/QRCodeDisplay.vue";
import Camera from "./components/Camera.vue";
import Header from "./components/Header.vue";
import { useQRCode } from "@vueuse/integrations/useQRCode";
import { ref } from "vue";

const text = ref("");
const qrText = ref("");
const scannedText = ref("Scanning...");

enum headerMessages {
  QRCodeGenerator = "QR Code Generator",
  QRCodeScanner = "QR Code Scanner",
}

const isGenerator = ref(true);
const headerMessage = ref(headerMessages.QRCodeGenerator);

const qrCode = useQRCode(qrText, {
  errorCorrectionLevel: "H",
  margin: 3,
  width: 500,
});

const updateText = (newText: string) => {
  text.value = newText;
};

const generateQRCode = () => {
  if (text.value.trim()) {
    qrText.value = text.value;
  }
};

const toggleAction = (value: boolean) => {
  isGenerator.value = value;
  headerMessage.value = value
    ? headerMessages.QRCodeGenerator
    : headerMessages.QRCodeScanner;
};

const showScannedText = (newText: string) => {
  scannedText.value = newText;
};
</script>

<template>
  <div class="app">
    <Header
      :message="headerMessage"
      @changeAction="toggleAction"
      :isGenerator="isGenerator"
    />

    <main class="main-content">
      <div class="container">
        <div class="content-wrapper" :class="{ 'scanner-mode': !isGenerator }">
          <!-- Generator Mode -->
          <div v-if="isGenerator" class="generator-section fade-in">
            <div class="section-header">
              <h2 class="section-title">Create QR Code</h2>
              <p class="section-subtitle">Enter your text and generate a beautiful QR code</p>
            </div>
            
            <div class="generator-content">
              <TextBox
                v-model:text="text"
                @update:text="updateText"
                @generateQRCode="generateQRCode"
                class="text-input-section"
              />
              <QRCodeDisplay 
                :qrCode="qrCode" 
                class="qr-display-section"
              />
            </div>
          </div>

          <!-- Scanner Mode -->
          <div v-if="!isGenerator" class="scanner-section fade-in">
            <div class="section-header">
              <h2 class="section-title">Scan QR Code</h2>
              <p class="section-subtitle">Point your camera at a QR code to scan it</p>
            </div>
            
            <div class="scanner-content">
              <Camera
                @update:scannedText="showScannedText"
                class="camera-section"
              />
              <div class="scan-result">
                <div class="result-header">
                  <h3>Scanned Content</h3>
                </div>
                <div class="result-content">
                  <p class="result-text">{{ scannedText }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
  padding: 2rem 0;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
}

.content-wrapper {
  width: 100%;
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
  animation: fadeIn 0.8s ease-out 0.2s both;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.5rem;
  letter-spacing: -0.025em;
}

.section-subtitle {
  font-size: 1.1rem;
  color: var(--text-muted);
  max-width: 600px;
  margin: 0 auto;
  line-height: 1.6;
}

.generator-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  align-items: start;
}

.text-input-section,
.qr-display-section {
  animation: fadeIn 0.8s ease-out 0.4s both;
}

.scanner-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}

.camera-section {
  animation: fadeIn 0.8s ease-out 0.4s both;
}

.scan-result {
  width: 100%;
  max-width: 600px;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 16px;
  overflow: hidden;
  animation: fadeIn 0.8s ease-out 0.6s both;
}

.result-header {
  background: var(--gradient-subtle);
  padding: 1rem 1.5rem;
  border-bottom: 1px solid var(--border-color);
}

.result-header h3 {
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--text-primary);
  margin: 0;
}

.result-content {
  padding: 1.5rem;
}

.result-text {
  font-size: 1rem;
  color: var(--text-secondary);
  line-height: 1.6;
  word-break: break-word;
  margin: 0;
  min-height: 1.5rem;
}

/* Mobile Responsive */
@media (max-width: 1024px) {
  .generator-content {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .section-title {
    font-size: 2.2rem;
  }
}

@media (max-width: 768px) {
  .main-content {
    padding: 1.5rem 0;
  }
  
  .container {
    padding: 0 1rem;
  }
  
  .section-header {
    margin-bottom: 2rem;
  }
  
  .section-title {
    font-size: 2rem;
  }
  
  .section-subtitle {
    font-size: 1rem;
  }
  
  .generator-content {
    gap: 1.5rem;
  }
  
  .scanner-content {
    gap: 1.5rem;
  }
}

@media (max-width: 480px) {
  .main-content {
    padding: 1rem 0;
  }
  
  .container {
    padding: 0 0.75rem;
  }
  
  .section-title {
    font-size: 1.75rem;
  }
  
  .section-subtitle {
    font-size: 0.9rem;
  }
  
  .result-content {
    padding: 1rem;
  }
}
</style>