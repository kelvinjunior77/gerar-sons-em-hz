<script setup>
defineProps({
  disabled: Boolean
})

defineEmits(['update:enabled', 'update:base', 'update:beat'])

const model = defineModel('enabled', { type: Boolean })
const baseModel = defineModel('base', { type: Number, default: 200 })
const beatModel = defineModel('beat', { type: Number, default: 10 })
</script>

<template>
  <div class="binaural-beats">
    <label>
      <input 
        type="checkbox" 
        v-model="model"
        :disabled="disabled"
      >
      Batimentos Binaurais
    </label>
    
    <div v-if="model" class="controls">
      <div>
        <label>Frequência Base (Hz):</label>
        <input 
          type="range" 
          min="100" 
          max="500" 
          step="1"
          v-model="baseModel"
          :disabled="disabled"
        >
        <span>{{ baseModel }} Hz</span>
      </div>
      
      <div>
        <label>Frequência de Batimento (Hz):</label>
        <input 
          type="range" 
          min="1" 
          max="30" 
          step="0.1"
          v-model="beatModel"
          :disabled="disabled"
        >
        <span>{{ beatModel.toFixed(1) }} Hz</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.binaural-beats {
  background: #f5f5f5;
  padding: 15px;
  border-radius: 8px;
}

.controls {
  display: grid;
  gap: 10px;
  margin-top: 10px;
}

label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

input[type="range"] {
  width: 100%;
}
</style>