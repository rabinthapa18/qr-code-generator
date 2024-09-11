<template>
  <div class="textbox-container">
    <textarea v-model="text" placeholder="Enter text to generate QR code" />
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";

const text = ref("");

// define props and emits
defineProps<{ text: string }>();
const emit = defineEmits(["update:text"]);

const emitUpdate = (newText: string) => {
  emit("update:text", newText);
};

// watch for changes in the text value
watch(text, (newText) => {
  emitUpdate(newText);
});
</script>

<style scoped>
.textbox-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.text-input {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
