<!-- src/components/PresetFrequenciesModal.vue -->
<script setup>
import Modal from './Modal.vue'

defineProps({
  isOpen: Boolean
})

const emit = defineEmits(['close', 'select'])

// Lista completa de frequências
const presetFrequencies = [
  { name: 'Lá Musical (440Hz)', value: 440, type: 'music' },
  { name: 'Dó (261.63Hz)', value: 261.63, type: 'music' },
  { name: 'Mi (329.63Hz)', value: 329.63, type: 'music' },
  { name: 'Sol (392Hz)', value: 392, type: 'music' },
  { name: 'Meditação Theta (4-8Hz)', value: 6, type: 'meditation' },
  { name: 'Meditação Alpha (8-12Hz)', value: 10, type: 'meditation' },
  { name: 'Foco Beta (14-30Hz)', value: 20, type: 'focus' }
]

const selectFrequency = (freq) => {
  emit('select', freq)
  emit('close')
}
</script>

<template>
  <Modal 
    :isOpen="isOpen" 
    title="Frequências"
    @close="$emit('close')"
  >
    <div class="space-y-3">
      <!-- Seção de Notas Musicais -->
      <div>
        <h4 class="font-medium text-gray-500 dark:text-gray-400 mb-2">Notas Musicais</h4>
        <div class="space-y-2 flex flex-col">
          <div>
            <button
            v-for="preset in presetFrequencies.filter(p => p.type === 'music')"
            :key="preset.value"
            @click="selectFrequency(preset.value)"
            class="preset-item p-2 m-1 bg-slate-700 rounded-md text-sm"
          >
            {{ preset.name }}
            
          </button>
          </div>
          
        </div>
      </div>

      <!-- Seção de Meditação -->
      <div>
        <h4 class="font-medium text-gray-500 dark:text-gray-400 mb-2">Meditação</h4>
        <div class="space-y-2 flex flex-col">
          <button
            v-for="preset in presetFrequencies.filter(p => p.type === 'meditation')"
            :key="preset.value"
            @click="selectFrequency(preset.value)"
            class="preset-item p-2 m-1 bg-slate-700 rounded-md text-sm"
          >
            {{ preset.name }}
          </button>
        </div>
      </div>
    </div>
  </Modal>
</template>

<style>
.preset-item {
  @apply w-full px-4 py-3 text-left hover:bg-gray-100 dark:hover:bg-gray-700 rounded-md transition-colors flex justify-between items-center;
}

.frequency-value {
  @apply bg-indigo-100 dark:bg-indigo-900 text-indigo-800 dark:text-indigo-200 px-2 py-1 rounded text-sm;
}
</style>