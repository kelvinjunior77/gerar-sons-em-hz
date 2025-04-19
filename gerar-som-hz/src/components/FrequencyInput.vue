<script setup>
defineProps({
  modelValue: {
    type: Number,
    required: true
  },
  disabled: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <div>
    <label for="frequency">Frequência (Hz):</label>
    <input
      type="number"
      id="frequency"
      :value="modelValue"
      @input="emit('update:modelValue', Number($event.target.value))"
      min="20"
      max="20000"
      step="1"
      :disabled="disabled"
    >
  </div>
</template>