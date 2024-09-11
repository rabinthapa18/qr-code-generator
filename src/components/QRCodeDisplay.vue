<template>
  <div class="qrcode-container">
    <img
      v-if="qrCode"
      class="qr-code"
      :src="qrCode"
      alt="QR Code"
      @click="toggleDialog(true)"
    />
    <note v-else>Enter text to display QR code</note>
  </div>

  <!-- dialog dor enlarged qrcode -->
  <div v-if="isDialogOpen" class="dialog-overlay" @click="toggleDialog(false)">
    <div
      class="dialog-content"
      @mouseenter="toggleDownloadButton(true)"
      @mouseleave="toggleDownloadButton(false)"
    >
      <div class="download-button-wrapper" v-if="isDownloadable">
        <button class="download-button" @click="downloadQRCode">
          Download
        </button>
      </div>
      <img :src="qrCode" alt="QR Code Enlarged" class="qr-code-enlarged" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps } from "vue";

// define props
const props = defineProps<{ qrCode: string }>();

// define data
const isDialogOpen = ref(false);
const isDownloadable = ref(false);

// define methods
const toggleDialog = (value: boolean) => {
  isDialogOpen.value = value;
};

const toggleDownloadButton = (value: boolean) => {
  isDownloadable.value = value;
  console.log("hovered");
};

const downloadQRCode = () => {
  const link = document.createElement("a");
  link.href = props.qrCode;
  link.download = "qrcode.png";
  link.click();
};
</script>

<style scoped>
.qrcode-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.qr-code {
  max-width: 20vw;
  height: auto;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
}

.note {
  font-size: 1.5rem;
  color: #ccc;
}

.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog-content {
  background-color: #fff;
  border-radius: 4px;
  position: relative;
}

.qr-code-enlarged {
  width: 100%;
  height: auto;
  cursor: pointer;
}

.download-button-wrapper {
  position: absolute;
  top: 0;
  right: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.download-button {
  background-color: #9251f9;
  color: #ffffff;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.download-button:hover {
  background-color: #7b3dbf;
}
</style>
