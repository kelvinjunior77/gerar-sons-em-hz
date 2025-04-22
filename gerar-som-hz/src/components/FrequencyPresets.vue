<script setup>
const props = defineProps({
  disabled: Boolean
})

const emit = defineEmits(['update:modelValue'])

const presets = [
  { label: 'Lá Musical (440Hz)', value: 440 },
  { label: 'Dó (261.63Hz)', value: 261.63 },
  { label: 'Mi (329.63Hz)', value: 329.63 },
  { label: 'Sol (392Hz)', value: 392 },
  { label: 'Meditação Theta (4-8Hz)', value: 6 },
  { label: 'Meditação Alpha (8-12Hz)', value: 10 },
  { label: 'Foco Beta (14-30Hz)', value: 20 }
]

const selectPreset = (value) => {
  emit('update:modelValue', value)
}
</script>

<template>
 

  <!-- Wave Type Selector -->
  <div>
    <label class="block text-sm font-medium mb-2 text-gray-300">Frequências Predefinidas</label>
    <div class="grid grid-cols-4 gap-2">

      <button v-for="preset in presets" :key="preset.value" @click="selectPreset(preset.value)" :disabled="disabled"
        class="bg-gray-700 hover:bg-gray-600 py-2 px-3 rounded-md text-xs font-medium transition-colors border border-transparent hover:border-primary/50">
        {{ preset.label }}
      </button>

    </div>
  </div>
</template>
