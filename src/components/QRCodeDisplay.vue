<template>
  <div class="qrcode-container">
    <img v-if="qrCode" class="qr-code" :src="qrCode" alt="QR Code" />
    <note v-else>Enter text to display QR code</note>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from "vue";

// define props
defineProps<{ qrCode: string }>();
</script>

<style scoped>
.qrcode-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.qr-code {
  max-width: 100%;
  height: auto;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
