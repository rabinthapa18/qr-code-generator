<template>
  <div class="header">
    <h1>{{ message }}</h1>
    <button @click="changeAction" class="action-button">
      {{ isGenerator ? "Switch to Scanner" : "Switch to Generator" }}
    </button>
  </div>
</template>
<script setup lang="ts">
// importing Vue Composition API functions
import { defineProps } from "vue";

const props = defineProps({
  message: String,
  isGenerator: Boolean,
});

// define emits
const emit = defineEmits(["changeAction"]);

// define functions
const changeAction = () => {
  emit("changeAction", !props.isGenerator);
};
</script>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  background-color: #9251f9;
  color: white;
}
h1 {
  margin: 0;
}
</style>
