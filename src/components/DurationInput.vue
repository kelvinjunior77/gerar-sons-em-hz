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
    <label for="duration" class="block text-sm font-medium mb-2 text-gray-300">Duração (segundos):</label>

    <div class="mt-2 flex items-center">
      <input type="number" id="duration" :value="modelValue"
        @input="emit('update:modelValue', Number($event.target.value))" min="1" max="10" step="1" :disabled="disabled" class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 focus:ring-2 focus:ring-primary focus:border-transparent">
    </div>

  </div>
</template>