<template>
  <header class="header">
    <div class="header-content">
      <div class="header-left">
        <div class="logo">
          <div class="logo-icon">
            <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M3 3h7v7H3V3zm11 0h7v7h-7V3zM3 14h7v7H3v-7zm11 0h7v7h-7v-7z" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <h1 class="logo-text">QR Studio</h1>
        </div>
        <div class="mode-indicator">
          <span class="mode-text">{{ message }}</span>
        </div>
      </div>
      
      <button @click="changeAction" class="toggle-button" :class="{ 'scanner-mode': !isGenerator }">
        <div class="button-content">
          <div class="icon-container">
            <svg v-if="isGenerator" class="icon" viewBox="0 0 24 24" fill="none">
              <path d="M9 12l2 2 4-4" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M21 12c-1 0-3-1-3-3s2-3 3-3 3 1 3 3-2 3-3 3" stroke="currentColor" stroke-width="2"/>
              <path d="M3 12c1 0 3-1 3-3s-2-3-3-3-3 1-3 3 2 3 3 3" stroke="currentColor" stroke-width="2"/>
            </svg>
            <svg v-else class="icon" viewBox="0 0 24 24" fill="none">
              <path d="M3 7V5a2 2 0 0 1 2-2h2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M17 3h2a2 2 0 0 1 2 2v2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M21 17v2a2 2 0 0 1-2 2h-2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M7 21H5a2 2 0 0 1-2-2v-2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <span class="button-text">
            {{ isGenerator ? "Scan QR" : "Generate QR" }}
          </span>
        </div>
        <div class="button-glow"></div>
      </button>
    </div>
  </header>
</template>

<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps({
  message: String,
  isGenerator: Boolean,
});

const emit = defineEmits(["changeAction"]);

const changeAction = () => {
  emit("changeAction", !props.isGenerator);
};
</script>

<style scoped>
.header {
  background: var(--gradient-primary);
  backdrop-filter: blur(20px);
  border-bottom: 1px solid var(--border-color);
  position: sticky;
  top: 0;
  z-index: 100;
  animation: slideIn 0.8s ease-out;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  max-width: 1400px;
  margin: 0 auto;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.logo-icon {
  width: 32px;
  height: 32px;
  color: white;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(10px);
}

.logo-text {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  margin: 0;
  letter-spacing: -0.025em;
}

.mode-indicator {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.15);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  backdrop-filter: blur(10px);
}

.mode-text {
  color: white;
  font-weight: 500;
  font-size: 0.9rem;
}

.toggle-button {
  position: relative;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: white;
  padding: 0.75rem 1.5rem;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  backdrop-filter: blur(10px);
  overflow: hidden;
  font-weight: 500;
}

.toggle-button:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
}

.toggle-button.scanner-mode {
  background: rgba(13, 148, 136, 0.2);
  border-color: rgba(13, 148, 136, 0.3);
}

.button-content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  position: relative;
  z-index: 2;
}

.icon-container {
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  width: 100%;
  height: 100%;
  transition: transform 0.3s ease;
}

.toggle-button:hover .icon {
  transform: scale(1.1);
}

.button-text {
  font-size: 0.9rem;
  white-space: nowrap;
}

.button-glow {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s ease;
}

.toggle-button:hover .button-glow {
  left: 100%;
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .header-content {
    padding: 1rem;
    gap: 1rem;
  }
  
  .header-left {
    gap: 1rem;
  }
  
  .logo-text {
    font-size: 1.25rem;
  }
  
  .mode-indicator {
    display: none;
  }
  
  .toggle-button {
    padding: 0.6rem 1.2rem;
  }
  
  .button-text {
    font-size: 0.85rem;
  }
}

@media (max-width: 480px) {
  .header-content {
    padding: 0.75rem;
  }
  
  .logo-icon {
    width: 28px;
    height: 28px;
  }
  
  .logo-text {
    font-size: 1.1rem;
  }
  
  .toggle-button {
    padding: 0.5rem 1rem;
  }
  
  .button-text {
    display: none;
  }
}
</style>