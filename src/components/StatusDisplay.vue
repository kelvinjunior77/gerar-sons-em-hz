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
  
  <div v-if="isPlaying" class="text-center bg-stone-300 rounded-md p-2 text-base font-medium">
    <p v-if="infinitePlay">Tocando infinito: {{ frequency }} Hz</p>
    <p v-else>Tocando: {{ frequency }} Hz por {{ duration }} segundos</p>
  </div>
</template>

<style scoped>
 
 p {
  color:#0f172a;
 }

</style>

