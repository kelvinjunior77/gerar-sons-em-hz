<script setup>
const props = defineProps({
  disabled: Boolean
})

const emit = defineEmits(['update:modelValue'])

const waveTypes = [
  { value: 'sine', label: 'Seno' },
  { value: 'square', label: 'Quadrada' },
  { value: 'sawtooth', label: 'Serra' },
  { value: 'triangle', label: 'Triangular' }
]
</script>

<template>
  <div class="wave-selector">
    <label>Tipo de Onda:</label>
    <select 
      :value="modelValue"
      @change="emit('update:modelValue', $event.target.value)"
      :disabled="disabled"
    >
      <option 
        v-for="type in waveTypes" 
        :key="type.value" 
        :value="type.value"
      >
        {{ type.label }}
      </option>
    </select>
  </div>
</template>

<style scoped>
.wave-selector {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

select {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}
</style>