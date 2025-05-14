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
  <div class="">
    <label for="frequency" class="block text-sm font-medium mb-2 text-gray-300">Frequência (Hz)</label>
    <div class="relative">


      <div class="mt-2 flex items-center">

        <input type="number" id="frequency" :value="modelValue"
          @input="emit('update:modelValue', Number($event.target.value))" min="20" max="20000" step="1"
          :disabled="disabled" class="w-full bg-gray-700 border border-gray-600 rounded-md px-3 py-2 text-center">
        <!---<span class="ml-2 text-gray-300">Hz</span>--->

      </div>
      <div class="flex justify-between text-xs text-gray-400 mt-1">
        <span>20Hz</span>
        <span>20kHz</span>
      </div>

    </div>

  </div>



</template>