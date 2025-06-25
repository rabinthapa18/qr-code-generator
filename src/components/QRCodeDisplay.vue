<template>
  <div class="qrcode-container">
    <div v-if="qrCode" class="qr-wrapper">
      <div class="qr-frame">
        <img
          class="qr-code"
          :src="qrCode"
          alt="Generated QR Code"
          @click="toggleDialog(true)"
        />
        <div class="qr-overlay">
          <button class="expand-button" @click="toggleDialog(true)">
            <svg viewBox="0 0 24 24" fill="none">
              <path d="M8 3H5a2 2 0 0 0-2 2v3m18 0V5a2 2 0 0 0-2-2h-3m0 18h3a2 2 0 0 0 2-2v-3M3 16v3a2 2 0 0 0 2 2h3" stroke="currentColor" stroke-width="2"/>
            </svg>
          </button>
        </div>
      </div>
      <div class="qr-info">
        <p class="qr-description">Click to enlarge and download</p>
      </div>
    </div>
    
    <div v-else class="empty-state">
      <div class="empty-icon">
        <svg viewBox="0 0 24 24" fill="none">
          <path d="M3 3h7v7H3V3zm11 0h7v7h-7V3zM3 14h7v7H3v-7z" stroke="currentColor" stroke-width="2"/>
          <rect x="14" y="14" width="2" height="2" fill="currentColor"/>
          <rect x="16" y="16" width="2" height="2" fill="currentColor"/>
          <rect x="18" y="14" width="2" height="2" fill="currentColor"/>
          <rect x="16" y="18" width="2" height="2" fill="currentColor"/>
          <rect x="18" y="20" width="2" height="2" fill="currentColor"/>
          <rect x="20" y="16" width="2" height="2" fill="currentColor"/>
        </svg>
      </div>
      <h3 class="empty-title">QR Code Preview</h3>
      <p class="empty-description">Your QR code will appear here once you enter some text</p>
    </div>
  </div>

  <!-- Enhanced Dialog -->
  <Teleport to="body">
    <div v-if="isDialogOpen" class="dialog-overlay" @click="toggleDialog(false)">
      <div class="dialog-content" @click.stop>
        <div class="dialog-header">
          <h3>QR Code</h3>
          <button class="close-button" @click="toggleDialog(false)">
            <svg viewBox="0 0 24 24" fill="none">
              <line x1="18" y1="6" x2="6" y2="18" stroke="currentColor" stroke-width="2"/>
              <line x1="6" y1="6" x2="18" y2="18" stroke="currentColor" stroke-width="2"/>
            </svg>
          </button>
        </div>
        
        <div class="dialog-body">
          <div class="qr-display">
            <img :src="qrCode" alt="QR Code Enlarged" class="qr-code-enlarged" />
          </div>
          
          <div class="dialog-actions">
            <button class="download-button" @click="downloadQRCode">
              <svg class="download-icon" viewBox="0 0 24 24" fill="none">
                <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4" stroke="currentColor" stroke-width="2"/>
                <polyline points="7,10 12,15 17,10" stroke="currentColor" stroke-width="2"/>
                <line x1="12" y1="15" x2="12" y2="3" stroke="currentColor" stroke-width="2"/>
              </svg>
              Download QR Code
            </button>
          </div>
        </div>
      </div>
    </div>
  </Teleport>
</template>

<script setup lang="ts">
import { ref, defineProps } from "vue";

const props = defineProps<{ qrCode: string }>();

const isDialogOpen = ref(false);

const toggleDialog = (value: boolean) => {
  isDialogOpen.value = value;
  if (value) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = '';
  }
};

const downloadQRCode = () => {
  const link = document.createElement("a");
  link.href = props.qrCode;
  link.download = `qrcode-${Date.now()}.png`;
  link.click();
};
</script>

<style scoped>
.qrcode-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  min-height: 300px;
}

.qr-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  animation: fadeIn 0.6s ease-out;
}

.qr-frame {
  position: relative;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 20px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  cursor: pointer;
  overflow: hidden;
}

.qr-frame:hover {
  border-color: var(--primary-purple);
  box-shadow: 0 8px 32px rgba(147, 51, 234, 0.2);
  transform: translateY(-4px);
}

.qr-code {
  width: 100%;
  max-width: 280px;
  height: auto;
  border-radius: 12px;
  transition: transform 0.3s ease;
}

.qr-frame:hover .qr-code {
  transform: scale(1.02);
}

.qr-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.3s ease;
  border-radius: 20px;
}

.qr-frame:hover .qr-overlay {
  opacity: 1;
}

.expand-button {
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 12px;
  padding: 1rem;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.expand-button:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

.expand-button svg {
  width: 24px;
  height: 24px;
}

.qr-info {
  text-align: center;
}

.qr-description {
  color: var(--text-muted);
  font-size: 0.9rem;
  margin: 0;
}

.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 3rem 2rem;
  background: var(--bg-secondary);
  border: 2px dashed var(--border-color);
  border-radius: 20px;
  min-height: 300px;
  width: 100%;
  max-width: 400px;
}

.empty-icon {
  width: 64px;
  height: 64px;
  color: var(--text-muted);
  margin-bottom: 1rem;
  opacity: 0.6;
}

.empty-icon svg {
  width: 100%;
  height: 100%;
}

.empty-title {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text-secondary);
  margin: 0 0 0.5rem 0;
}

.empty-description {
  color: var(--text-muted);
  font-size: 0.9rem;
  line-height: 1.5;
  margin: 0;
  max-width: 280px;
}

/* Dialog Styles */
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1rem;
  backdrop-filter: blur(8px);
  animation: fadeIn 0.3s ease-out;
}

.dialog-content {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 20px;
  max-width: 500px;
  width: 100%;
  max-height: 90vh;
  overflow: hidden;
  animation: slideIn 0.3s ease-out;
}

.dialog-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem;
  border-bottom: 1px solid var(--border-color);
  background: var(--gradient-subtle);
}

.dialog-header h3 {
  font-size: 1.25rem;
  font-weight: 600;
  color: var(--text-primary);
  margin: 0;
}

.close-button {
  background: none;
  border: none;
  color: var(--text-muted);
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.close-button:hover {
  background: rgba(255, 255, 255, 0.1);
  color: var(--text-primary);
}

.close-button svg {
  width: 20px;
  height: 20px;
}

.dialog-body {
  padding: 2rem;
}

.qr-display {
  display: flex;
  justify-content: center;
  margin-bottom: 2rem;
}

.qr-code-enlarged {
  width: 100%;
  max-width: 300px;
  height: auto;
  border-radius: 12px;
}

.dialog-actions {
  display: flex;
  justify-content: center;
}

.download-button {
  background: var(--gradient-primary);
  border: none;
  border-radius: 12px;
  padding: 1rem 2rem;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  box-shadow: 0 4px 15px rgba(147, 51, 234, 0.3);
}

.download-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(147, 51, 234, 0.4);
}

.download-icon {
  width: 20px;
  height: 20px;
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .qr-frame {
    padding: 1.25rem;
  }
  
  .qr-code {
    max-width: 240px;
  }
  
  .empty-state {
    padding: 2rem 1.5rem;
    min-height: 250px;
  }
  
  .empty-icon {
    width: 48px;
    height: 48px;
  }
  
  .dialog-content {
    margin: 1rem;
    max-width: calc(100% - 2rem);
  }
  
  .dialog-body {
    padding: 1.5rem;
  }
  
  .qr-code-enlarged {
    max-width: 250px;
  }
}

@media (max-width: 480px) {
  .qr-frame {
    padding: 1rem;
  }
  
  .qr-code {
    max-width: 200px;
  }
  
  .empty-state {
    padding: 1.5rem 1rem;
    min-height: 200px;
  }
  
  .dialog-header {
    padding: 1rem;
  }
  
  .dialog-body {
    padding: 1rem;
  }
  
  .download-button {
    padding: 0.875rem 1.5rem;
    font-size: 0.9rem;
  }
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: scale(0.9) translateY(20px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}
</style>