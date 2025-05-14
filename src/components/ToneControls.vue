<script setup>
import { ref, onMounted } from 'vue';

defineProps({
  isPlaying: {
    type: Boolean,
    required: true
  },

  isRecording: Boolean,

  infinitePlay: {
    type: Boolean,
    required: true
  }
})

const emit = defineEmits(['play', 'stop', 'toggle-infinite'])

var tipo = ref(false)

const mostrar = () => {
    tipo.value = true
}
onMounted(() => {
    setTimeout(() => {
      tipo.value = false
    }, 3000);
})

</script>

<template>
  <!-- Controls -->
  <div class="flex flex-wrap gap-3 pt-2">

    <button class="bg-primary hover:bg-primary/90 px-2 py-2.5 rounded-lg font-medium text-sm flex items-center transition-all"
      @click="emit('play')" :disabled="isPlaying">
      <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"></path>
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z">
        </path>
      </svg>
      Reproduzir
    </button>

    <button class="bg-primary hover:bg-primary/90 px-2 py-2.5 rounded-lg font-medium text-sm flex items-center transition-all"
      @click="emit('toggle-infinite')" :disabled="isPlaying && !infinitePlay">
      <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z"></path>
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z">
        </path>
      </svg>
      {{ infinitePlay ? 'Parar Infinito' : 'Reproduzir Infinito' }}
    </button>

    <button class="bg-gray-700 hover:bg-gray-600 px-2 py-2.5 rounded-lg font-medium text-sm  flex items-center transition-all"
      @click="emit('stop')" :disabled="!isPlaying">
      <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z">
        </path>
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
          d="M9 10a1 1 0 011-1h4a1 1 0 011 1v4a1 1 0 01-1 1h-4a1 1 0 01-1-1v-4z"></path>
      </svg>
      Parar
    </button>

    <slot name="gravacao">

    </slot>
    <span v-if="isRecording"
      class="bg-gray-700 hover:bg-gray-600 px-5 py-2.5 rounded-lg font-medium flex items-center transition-all">
      ● Gravando
    </span>
    <!---<button class="border border-primary text-primary hover:bg-primary/10 px-5 py-2.5 rounded-lg font-medium flex items-center transition-all">
              <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"></path>
              </svg>
              Gravar
            </button>
            <button class="bg-secondary hover:bg-secondary/90 px-5 py-2.5 rounded-lg font-medium flex items-center transition-all ml-auto">
              <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path>
              </svg>
              Exportar
            </button> --->
  </div>

</template>
