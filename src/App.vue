<script setup lang="ts">
// importing custom components
import TextBox from "./components/TextBox.vue";
import QRCodeDisplay from "./components/QRCodeDisplay.vue";

// importing external libraries
import { useQRCode } from "@vueuse/integrations/useQRCode";

import { ref, watch } from "vue";

const text = ref("");

const updateText = (newText: string) => {
  text.value = newText;
};

var qrCode = useQRCode("");

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
  <div class="container">
    <TextBox v-model:text="text" @update:text="updateText" class="textbox" />
    <QRCodeDisplay :qrCode="qrCode" class="qrcode-display" />
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
