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
      :isDisabled="!isGenerator"
      @update:text="updateText"
      class="textbox"
    />
    <QRCodeDisplay v-if="isGenerator" :qrCode="qrCode" class="qrcode-display" />
    <TextBox
      v-if="!isGenerator"
      v-model:text="text"
      :isDisabled="true"
      @update:text="updateText"
      class="textbox"
    />
    <Camera v-if="!isGenerator" class="qrcode-display" />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
  align-items: start;
  width: 100%;
}

.textbox,
.qrcode-display {
  min-width: 48%;
  box-sizing: border-box;
}
</style>
