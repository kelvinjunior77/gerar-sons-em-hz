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
    <label for="duration">Duração (segundos):</label>
    <input
      type="number"
      id="duration"
      :value="modelValue"
      @input="emit('update:modelValue', Number($event.target.value))"
      min="1"
      max="10"
      step="1"
      :disabled="disabled"
    >
  </div>
</template>