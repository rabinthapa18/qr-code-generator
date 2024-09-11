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
    <div class="dialog-content">
      <img :src="qrCode" alt="QR Code Enlarged" class="qr-code-enlarged" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, defineProps } from "vue";

// define props
defineProps<{ qrCode: string }>();

// define data
const isDialogOpen = ref(false);

// define methods
const toggleDialog = (value: boolean) => {
  isDialogOpen.value = value;
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
}

.qr-code-enlarged {
  width: 100%;
  height: auto;
}
</style>
