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
  <div class="presets">
    <h3>Frequências Predefinidas:</h3>
    <div class="preset-buttons">
      <button
        v-for="preset in presets"
        :key="preset.value"
        @click="selectPreset(preset.value)"
        :disabled="disabled"
      >
        {{ preset.label }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.presets {
  background: #f5f5f5;
  padding: 15px;
  border-radius: 8px;
}

h3 {
  margin-top: 0;
  margin-bottom: 10px;
}

.preset-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

button {
  padding: 8px 12px;
  background: #e0e0e0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover:not(:disabled) {
  background: #d0d0d0;
}

button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
</style>