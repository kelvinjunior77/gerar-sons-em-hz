<!-- src/components/LoadingSpinner.vue -->
<script setup>
defineProps({
  message: {
    type: String,
    default: "Carregando..."
  },
  progress: {
    type: Number,
    default: null
  }
})
</script>

<template>
  <div class="loading-overlay">
    <div class="loading-container">
      <div class="loading-spinner"></div>
      
      <p class="loading-message">{{ message }}</p>
      
      <div v-if="progress !== null" class="progress-bar bg-indigo-500 ">
        <div class="progress" :style="{ width: `${progress}%` }"></div>
      </div>
    </div>
  </div>

  
</template>

<style scoped>
.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255, 255, 255, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  backdrop-filter: blur(3px);
}

.loading-container {
  text-align: center;
  max-width: 300px;
  padding: 30px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.loading-spinner {
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #2c3e50;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

.loading-message {
  margin-bottom: 20px;
  color: #2c3e50;
  font-size: 16px;
}

.progress-bar {
  height: 6px;
  background: #e0e0e0;
  border-radius: 3px;
  overflow: hidden;
  margin-top: 15px;
}

.progress {
  height: 100%;
  background: linear-gradient(90deg, #42b883, #35495e);
  transition: width 0.3s ease;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Transições */
.loading-enter-active,
.loading-leave-active {
  transition: opacity 0.5s ease;
}

.loading-enter-from,
.loading-leave-to {
  opacity: 0;
}
</style>