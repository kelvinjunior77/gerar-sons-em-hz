<script setup>
defineProps({
  isPlaying: {
    type: Boolean,
    required: true
  },
  infinitePlay: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['play', 'stop', 'toggle-infinite'])
</script>

<template>
  <div class="controls">
    <button class="btn-play" @click="emit('play')" :disabled="isPlaying">Reproduzir</button>
    <button class="btn-stop" @click="emit('stop')" :disabled="!isPlaying">Parar</button>
    <button class="btn-infinite" @click="emit('toggle-infinite')" :disabled="isPlaying && !infinitePlay">
      {{ infinitePlay ? 'Parar Infinito' : 'Reproduzir Infinito' }}
    </button>
  </div>
</template>

<style scoped>
.controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

button:disabled {
  background-color: #cccccc;
}

.btn-play {
  background-color: #4CAF50;
  color: white;
}

.btn-stop {
  background-color: #f44336;
  color: white;
}

.btn-infinite {
  background-color: #2196F3;
  color: white;
}
</style>