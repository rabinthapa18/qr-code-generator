<template>
  <div class="textbox-container">
    <textarea
      cols="50"
      rows="10"
      v-model="text"
      placeholder="Enter text to generate QR code"
      :disabled="isDisabled"
    />
    <button v-if="!isDisabled" @click="generateQRCode" class="generate-button">
      Generate QR Code
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const text = ref("");

// define props and emits
defineProps<{
  text: string;
  isDisabled: boolean;
}>();
const emit = defineEmits(["update:text", "generateQRCode"]);

const emitUpdate = (newText: string) => {
  emit("update:text", newText);
};

const generateQRCode = () => {
  emitUpdate(text.value);
};
</script>

<style scoped>
.textbox-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  gap: 1rem;
}

.text-input {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.generate-button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  background-color: #9251f9;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.generate-button:hover {
  background-color: #7b3dbf;
}
</style>
