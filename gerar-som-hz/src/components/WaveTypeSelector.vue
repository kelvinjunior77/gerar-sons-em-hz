<script setup>
const props = defineProps({
  disabled: Boolean
})

const emit = defineEmits(['update:modelValue'])

const waveTypes = [
  { id: 0, value: 'sine', label: 'Seno' },
  { id: 1, value: 'square', label: 'Quadrada' },
  { id: 2, value: 'sawtooth', label: 'Serra' },
  { id: 3, value: 'triangle', label: 'Triangular' }
]
</script>

<template>
  
  
<div class="mb-4">
  <label for="waveType" class="block text-sm font-medium mb-2 text-gray-700 dark:text-gray-300">
    Tipo de Onda
  </label>
  <select :value="modelValue" @change="emit('update:modelValue', $event.target.value)"
    id="waveType" 
    v-model="waveType"
    class="w-full px-3 py-2 bg-white dark:bg-gray-800 border border-gray-300 dark:border-gray-600 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-primary-500 text-gray-900 dark:text-gray-100 transition-colors"
  >
    <option v-for="type in waveTypes" :key="type.id" :value="type.value" class="bg-white dark:bg-gray-800">
      {{ type.label }}</option>
   
  </select>
</div>


</template>
