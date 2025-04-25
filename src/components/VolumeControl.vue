<script setup>
const props = defineProps({
  disabled: Boolean
})

const emit = defineEmits(['update:modelValue'])
</script>

<template>
    <!---
    <label> Volume: {{ (modelValue * 100).toFixed(0) }} %  </label>--->

  <div>
    <label for="volume" class="block text-sm font-medium mb-2 text-gray-300">Volume</label>
    <div class="flex items-center space-x-4">
      <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z">
        </path>
      </svg>
      <input type="range" min="0" max="1" step="0.01" :value="modelValue"
        @input="emit('update:modelValue', parseFloat($event.target.value))" :disabled="disabled"
        class="flex-1 h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer accent-primary">
      <svg class="w-5 h-5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M15.536 8.464a5 5 0 010 7.072M12 6a7.975 7.975 0 015.657 2.343m0 0a7.975 7.975 0 010 11.314m-11.314 0a7.975 7.975 0 010-11.314m0 0a7.975 7.975 0 015.657-2.343">
        </path>
      </svg>
    </div>
  </div>
</template>
