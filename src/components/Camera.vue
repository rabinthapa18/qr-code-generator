<template>
  <div class="camera-container">
    <div class="camera-wrapper">
      <div class="camera-frame">
        <video ref="videoElement" class="video" autoplay playsinline></video>
        <div class="scan-overlay">
          <div class="scan-corners">
            <div class="corner top-left"></div>
            <div class="corner top-right"></div>
            <div class="corner bottom-left"></div>
            <div class="corner bottom-right"></div>
          </div>
          <div class="scan-line"></div>
        </div>
        <div class="camera-controls">
          <div class="status-indicator" :class="{ 'scanning': isScanning }">
            <div class="status-dot"></div>
            <span class="status-text">{{ isScanning ? 'Scanning...' : 'Ready' }}</span>
          </div>
        </div>
      </div>
      <div class="camera-info">
        <p class="camera-description">Position the QR code within the frame to scan</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, defineEmits, onBeforeUnmount } from "vue";
import { BrowserQRCodeReader } from "@zxing/browser";

const videoElement = ref<HTMLVideoElement | null>(null);
const isScanning = ref(false);
let codeReader: BrowserQRCodeReader | null = null;

const emit = defineEmits(["update:scannedText"]);

onMounted(() => {
  codeReader = new BrowserQRCodeReader();
  isScanning.value = true;

  navigator.mediaDevices
    .getUserMedia({ 
      video: { 
        facingMode: "environment",
        width: { ideal: 1280 },
        height: { ideal: 720 }
      } 
    })
    .then((stream) => {
      if (videoElement.value) {
        videoElement.value.srcObject = stream;

        codeReader?.decodeFromVideoDevice(
          undefined,
          videoElement.value,
          (result, error) => {
            if (result) {
              emit("update:scannedText", result.getText());
              // Brief pause in scanning animation when QR is detected
              isScanning.value = false;
              setTimeout(() => {
                isScanning.value = true;
              }, 1000);
            }
            if (error && error.name !== 'NotFoundException') {
              console.warn("QR Code scanning error: ", error);
            }
          }
        );
      }
    })
    .catch((err) => {
      console.error("Error accessing camera: ", err);
      isScanning.value = false;
      emit("update:scannedText", "Camera access denied or not available");
    });
});

onBeforeUnmount(() => {
  const stream = videoElement.value?.srcObject as MediaStream;
  
  stream?.getTracks().forEach((track) => {
    track.stop();
  });

  if (videoElement.value) {
    videoElement.value.srcObject = null;
  }
  
  isScanning.value = false;
});
</script>

<style scoped>
.camera-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.camera-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  animation: fadeIn 0.6s ease-out;
}

.camera-frame {
  position: relative;
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 20px;
  padding: 1rem;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.video {
  width: 100%;
  max-width: 400px;
  height: 300px;
  object-fit: cover;
  border-radius: 12px;
  background: var(--bg-tertiary);
}

.scan-overlay {
  position: absolute;
  top: 1rem;
  left: 1rem;
  right: 1rem;
  bottom: 1rem;
  pointer-events: none;
  border-radius: 12px;
}

.scan-corners {
  position: absolute;
  top: 20%;
  left: 20%;
  right: 20%;
  bottom: 20%;
}

.corner {
  position: absolute;
  width: 30px;
  height: 30px;
  border: 3px solid var(--primary-purple);
  border-radius: 4px;
}

.corner.top-left {
  top: 0;
  left: 0;
  border-right: none;
  border-bottom: none;
}

.corner.top-right {
  top: 0;
  right: 0;
  border-left: none;
  border-bottom: none;
}

.corner.bottom-left {
  bottom: 0;
  left: 0;
  border-right: none;
  border-top: none;
}

.corner.bottom-right {
  bottom: 0;
  right: 0;
  border-left: none;
  border-top: none;
}

.scan-line {
  position: absolute;
  top: 20%;
  left: 20%;
  right: 20%;
  height: 2px;
  background: var(--gradient-primary);
  border-radius: 1px;
  animation: scanLine 2s ease-in-out infinite;
  box-shadow: 0 0 10px rgba(147, 51, 234, 0.5);
}

.camera-controls {
  position: absolute;
  bottom: 1rem;
  left: 1rem;
  right: 1rem;
  display: flex;
  justify-content: center;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(0, 0, 0, 0.7);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--secondary-teal);
  transition: all 0.3s ease;
}

.status-indicator.scanning .status-dot {
  background: var(--primary-purple);
  animation: pulse 1.5s ease-in-out infinite;
}

.status-text {
  color: white;
  font-size: 0.8rem;
  font-weight: 500;
}

.camera-info {
  text-align: center;
}

.camera-description {
  color: var(--text-muted);
  font-size: 0.9rem;
  margin: 0;
  max-width: 300px;
  line-height: 1.5;
}

@keyframes scanLine {
  0% {
    top: 20%;
    opacity: 1;
  }
  50% {
    top: 70%;
    opacity: 0.8;
  }
  100% {
    top: 20%;
    opacity: 1;
  }
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.7;
  }
}

/* Mobile Responsive */
@media (max-width: 768px) {
  .video {
    max-width: 350px;
    height: 260px;
  }
  
  .corner {
    width: 25px;
    height: 25px;
    border-width: 2px;
  }
  
  .camera-description {
    font-size: 0.85rem;
  }
}

@media (max-width: 480px) {
  .camera-frame {
    padding: 0.75rem;
  }
  
  .video {
    max-width: 300px;
    height: 220px;
  }
  
  .corner {
    width: 20px;
    height: 20px;
  }
  
  .scan-corners {
    top: 25%;
    left: 25%;
    right: 25%;
    bottom: 25%;
  }
  
  .status-indicator {
    padding: 0.4rem 0.8rem;
  }
  
  .status-text {
    font-size: 0.75rem;
  }
}
</style>