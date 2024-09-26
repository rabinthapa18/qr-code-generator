<template>
  <div class="camera">
    <video ref="videoElement" class="video" autoplay></video>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref, defineEmits, onBeforeUnmount } from "vue";
import { BrowserQRCodeReader } from "@zxing/browser";

// refs for the video element and QR code
const videoElement = ref<HTMLVideoElement | null>(null);
let codeReader: BrowserQRCodeReader | null = null;

const emit = defineEmits(["update:scannedText"]);

onMounted(() => {
  // initializing the QR code reader
  codeReader = new BrowserQRCodeReader();

  // requesting access to the user's camera
  navigator.mediaDevices
    .getUserMedia({ video: { facingMode: "environment" } })
    .then((stream) => {
      if (videoElement.value) {
        videoElement.value.srcObject = stream;

        codeReader?.decodeFromVideoDevice(
          undefined,
          videoElement.value,
          (result, error) => {
            if (result) {
              emit("update:scannedText", result.getText());
            }
            if (error) {
              console.warn("QR Code scanning error: ", error);
            }
          }
        );
      }
    })
    .catch((err) => {
      console.error("Error accessing camera: ", err);
    });
});

onBeforeUnmount(() => {
  const stream = videoElement.value?.srcObject as MediaStream;

  stream?.getTracks().forEach((track) => {
    track.stop();
  });

  videoElement.value!.srcObject = null;
});
</script>

<style scoped>
.camera {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: black;
  border-radius: 15px;
  overflow: hidden;
}

.video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
