<template>
  <div class="textbox-container">
    <div class="input-wrapper">
      <div class="input-header">
        <label for="text-input" class="input-label">
          <svg class="label-icon" viewBox="0 0 24 24" fill="none">
            <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z" stroke="currentColor" stroke-width="2"/>
            <polyline points="14,2 14,8 20,8" stroke="currentColor" stroke-width="2"/>
            <line x1="16" y1="13" x2="8" y2="13" stroke="currentColor" stroke-width="2"/>
            <line x1="16" y1="17" x2="8" y2="17" stroke="currentColor" stroke-width="2"/>
          </svg>
          Enter your text
        </label>
        <div class="char-counter" :class="{ 'warning': text.length > 400, 'danger': text.length >= 500 }">
          {{ text.length }} / 500
        </div>
      </div>
      
      <div class="textarea-wrapper">
        <textarea
          id="text-input"
          v-model="text"
          placeholder="Type your message, URL, or any text you want to convert to QR code..."
          class="text-input"
          maxlength="500"
          rows="8"
          @input="handleInput"
        />
        <div class="input-glow"></div>
      </div>
    </div>
    
    <button 
      @click="generateQRCode" 
      class="generate-button"
      :disabled="!text.trim()"
      :class="{ 'clicked': isClicked }"
    >
      <div class="button-content">
        <svg class="button-icon" viewBox="0 0 24 24" fill="none">
          <path d="M3 3h7v7H3V3zm11 0h7v7h-7V3zM3 14h7v7H3v-7z" stroke="currentColor" stroke-width="2"/>
          <rect x="14" y="14" width="2" height="2" fill="currentColor"/>
          <rect x="16" y="16" width="2" height="2" fill="currentColor"/>
          <rect x="18" y="14" width="2" height="2" fill="currentColor"/>
          <rect x="16" y="18" width="2" height="2" fill="currentColor"/>
          <rect x="18" y="20" width="2" height="2" fill="currentColor"/>
          <rect x="20" y="16" width="2" height="2" fill="currentColor"/>
        </svg>
        <span>Generate QR Code</span>
      </div>
      <div class="button-shimmer"></div>
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const text = ref("");
const isClicked = ref(false);

defineProps<{
  text: string;
}>();

const emit = defineEmits(["update:text", "generateQRCode"]);

const handleInput = () => {
  emit("update:text", text.value);
};

const generateQRCode = () => {
  if (text.value.trim()) {
    isClicked.value = true;
    emit("generateQRCode");
    
    // Remove the clicked class after animation
    setTimeout(() => {
      isClicked.value = false;
    }, 1000);
  }
};
</script>

<style scoped>
.textbox-container {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  width: 100%;
}

.input-wrapper {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 16px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.input-wrapper:hover {
  border-color: rgba(147, 51, 234, 0.3);
  box-shadow: 0 8px 32px rgba(147, 51, 234, 0.1);
}

.input-wrapper:focus-within {
  border-color: var(--primary-purple);
  box-shadow: 0 0 0 3px rgba(147, 51, 234, 0.1);
}

.input-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.input-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 600;
  color: var(--text-primary);
  font-size: 0.9rem;
}

.label-icon {
  width: 18px;
  height: 18px;
  color: var(--primary-purple);
}

.char-counter {
  font-size: 0.8rem;
  color: var(--text-muted);
  font-weight: 500;
  transition: color 0.3s ease;
}

.char-counter.warning {
  color: #f59e0b;
}

.char-counter.danger {
  color: #ef4444;
}

.textarea-wrapper {
  position: relative;
}

.text-input {
  width: 100%;
  background: var(--bg-tertiary);
  border: 1px solid var(--border-color);
  border-radius: 12px;
  padding: 1rem;
  font-size: 1rem;
  color: var(--text-primary);
  line-height: 1.6;
  resize: vertical;
  min-height: 120px;
  transition: all 0.3s ease;
  font-family: inherit;
}

.text-input::placeholder {
  color: var(--text-muted);
}

.text-input:focus {
  outline: none;
  border-color: var(--primary-purple);
  box-shadow: 0 0 0 3px rgba(147, 51, 234, 0.1);
}

.input-glow {
  position: absolute;
  top: -2px;
  left: -2px;
  right: -2px;
  bottom: -2px;
  background: var(--gradient-primary);
  border-radius: 14px;
  opacity: 0;
  transition: opacity 0.3s ease;
  z-index: -1;
}

.text-input:focus + .input-glow {
  opacity: 0.2;
}

.generate-button {
  background: var(--gradient-primary);
  border: none;
  border-radius: 12px;
  padding: 1rem 2rem;
  color: white;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(147, 51, 234, 0.3);
}

.generate-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(147, 51, 234, 0.4);
}

.generate-button:active:not(:disabled) {
  transform: translateY(0);
}

.generate-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.generate-button.clicked {
  background: var(--gradient-secondary);
  animation: gradientShift 1s ease-out;
}

.button-content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  position: relative;
  z-index: 2;
}

.button-icon {
  width: 20px;
  height: 20px;
  transition: transform 0.3s ease;
}

.generate-button:hover:not(:disabled) .button-icon {
  transform: scale(1.1) rotate(5deg);
}

.button-shimmer {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.6s ease;
}

.generate-button:hover:not(:disabled) .button-shimmer {
  left: 100%;
}

@keyframes gradientShift {
  0% {
    background: var(--gradient-primary);
  }
  50% {
    background: var(--gradient-secondary);
  }
  100% {
    background: var(--gradient-primary);
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .input-wrapper {
    padding: 1.25rem;
  }
  
  .input-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
  }
  
  .char-counter {
    align-self: flex-end;
  }
  
  .text-input {
    min-height: 100px;
    font-size: 0.95rem;
  }
  
  .generate-button {
    padding: 0.875rem 1.5rem;
    font-size: 0.95rem;
  }
}

@media (max-width: 480px) {
  .input-wrapper {
    padding: 1rem;
  }
  
  .text-input {
    min-height: 80px;
    padding: 0.875rem;
  }
  
  .generate-button {
    padding: 0.75rem 1.25rem;
    font-size: 0.9rem;
  }
  
  .button-content {
    gap: 0.5rem;
  }
  
  .button-icon {
    width: 18px;
    height: 18px;
  }
}
</style>