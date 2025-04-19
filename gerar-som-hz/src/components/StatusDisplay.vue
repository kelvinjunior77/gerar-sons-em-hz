<script setup>
defineProps({
  isPlaying: {
    type: Boolean,
    required: true
  },
  infinitePlay: {
    type: Boolean,
    required: true
  },
  frequency: {
    type: Number,
    required: true
  },
  duration: {
    type: Number,
    required: true
  },
  error: {
    type: String,
    default: null
  }
})
</script>

<template>
  <div v-if="error" class="error status">
    {{ error }}
  </div>
  
  <div v-if="isPlaying" class="playing status">
    <p v-if="infinitePlay">Tocando infinito: {{ frequency }} Hz</p>
    <p v-else>Tocando: {{ frequency }} Hz por {{ duration }} segundos</p>
  </div>
</template>

<style scoped>
.status {
  margin-top: 10px;
  padding: 10px;
  border-radius: 4px;
}

.playing {
  background-color: #e8f5e9;
}

.error {
  background-color: #ffebee;
  color: #f44336;
}
</style>