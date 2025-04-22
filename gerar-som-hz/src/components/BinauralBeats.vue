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
  <div class="">
    <label class="block text-sm font-medium mb-2 text-gray-300">
      <input 
        type="checkbox" 
        v-model="model"
        :disabled="disabled" class="cursor-pointer"
      >
      Batimentos Binaurais 
    </label>
    
    <div v-if="model" class="">
      <div>
        <label class=" flex justify-between text-xs text-gray-400 mt-1">Frequência Base (Hz):</label>
        <input 
          type="range" 
          min="100" 
          max="500" 
          step="1"
          v-model="baseModel"
          :disabled="disabled" class="flex-1 h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-primary"
        >
        <span class="text-xs m-1">{{ baseModel }} Hz</span>
      </div>
      
      <div>
        <label class="flex justify-between text-xs text-gray-400 mt-1">Frequência de Batimento (Hz):</label>
        <input 
          type="range" 
          min="1" 
          max="30" 
          step="0.1"
          v-model="beatModel"
          :disabled="disabled" class="flex-1 h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-primary"
        >
        <span class="text-xs m-1">{{ beatModel.toFixed(1) }} Hz</span>
      </div>
    </div>
  </div>
</template>

