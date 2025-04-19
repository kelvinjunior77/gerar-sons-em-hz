<script setup>
const props = defineProps({
  disabled: Boolean
})

const emit = defineEmits(['update:modelValue'])
</script>

<template>
  <div class="volume-control">

    <!---
    <label> Volume: {{ (modelValue * 100).toFixed(0) }} %  </label>--->

    <input
      type="range"
      min="0"
      max="1"
      step="0.01"
      :value="modelValue"
      @input="emit('update:modelValue', parseFloat($event.target.value))"
      :disabled="disabled"
    >
  </div>
</template>

<style scoped>
.volume-control {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

input[type="range"] {
  width: 100%;
}
</style>