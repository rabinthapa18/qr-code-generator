<script setup lang="ts">
// importing custom components
import TextBox from "./components/TextBox.vue";
import QRCodeDisplay from "./components/QRCodeDisplay.vue";
import Camera from "./components/Camera.vue";
import Header from "./components/Header.vue";

// importing external libraries
import { useQRCode } from "@vueuse/integrations/useQRCode";

// importing Vue Composition API functions
import { ref, watch } from "vue";

// define variables
const text = ref("");
var scannedText = ref("Scanning...");
var qrCode = useQRCode("");
enum headerMessages {
  QRCodeGenerator = "QR Code Generator",
  QRCodeScanner = "QR Code Scanner",
}
var isGenerator = true;
var headerMessage = ref(headerMessages.QRCodeGenerator);

// define functions
const updateText = (newText: string) => {
  text.value = newText;
};

const toggleAction = (value: boolean) => {
  isGenerator = value;
  headerMessage.value = isGenerator
    ? headerMessages.QRCodeGenerator
    : headerMessages.QRCodeScanner;
};

const showScannedText = (newText: string) => {
  scannedText.value = newText;
};

// watch for changes in the text value
watch(text, () => {
  qrCode = useQRCode(text, {
    errorCorrectionLevel: "H",
    margin: 3,
    width: 500,
  });
});
</script>

<template>
  <Header
    :message="headerMessage"
    @changeAction="toggleAction"
    :isGenerator="isGenerator"
  />

  <div class="container">
    <TextBox
      v-if="isGenerator"
      v-model:text="text"
      @update:text="updateText"
      class="textbox"
    />
    <QRCodeDisplay v-if="isGenerator" :qrCode="qrCode" class="qrcode-display" />
    <Camera
      v-if="!isGenerator"
      @update:scannedText="showScannedText"
      class="qrcode-display"
    />
    <p v-if="!isGenerator" class="qr-result">QR Code: {{ scannedText }}</p>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
  align-items: start;
  width: 100%;
  padding: 20px;
}

.textbox,
.qrcode-display {
  min-width: 48%;
  box-sizing: border-box;
}
.qr-result {
  position: absolute;
  bottom: 20px;
  color: white;
  font-size: 1.2rem;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 10px;
  border-radius: 8px;
}
</style>
