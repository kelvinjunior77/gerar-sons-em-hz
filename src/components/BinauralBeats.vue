<script setup>
import { ref, watch } from 'vue'
import Modal from './Modal.vue'

const props = defineProps({
  binauralActive: Boolean
})

const emit = defineEmits([
  'update:binauralActive',
  'frequency-update'
])

const showPanel = ref(false)
const baseFreq = ref(200)
const beatFreq = ref(10)

// Presets melhorados
const presets = [
  {
    name: "Relaxamento Profundo",
    base: 150,
    beat: 4,
    icon: "🌊",
    desc: "Ondas Theta (4-8Hz) - Ideal para meditação"
  },
  {
    name: "Foco Criativo",
    base: 300,
    beat: 14,
    icon: "🎯",
    desc: "Ondas Beta (14-30Hz) - Melhora a concentração"
  }
]

// Aplica um preset
const applyPreset = (preset) => {
  baseFreq.value = preset.base
  beatFreq.value = preset.beat
  activateBinaural()
}

// Ativa os batimentos
const activateBinaural = () => {
  emit('update:binauralActive', true)
  emit('frequency-update', baseFreq.value)
  showPanel.value = false
}

// Desativa os batimentos
const deactivateBinaural = () => {
  emit('update:binauralActive', false)
}

// Sincroniza com o estado externo
watch(() => props.binauralActive, (newVal) => {
  if (!newVal) {
    showPanel.value = false
  }
})
</script>

<template>
  <div class="binaural-wrapper">
    <!-- Botão Principal -->
    <button
      @click="showPanel = true"
      :class="{ 'active': binauralActive }"
      class="p-2 bg-slate-700 rounded-md text-sm font-bold "
    >
      Batimentos Binaurais
      
      <span v-if="binauralActive" class="p-2">
        - {{ baseFreq }}Hz ±{{ beatFreq }}Hz
      </span>
    </button>
    

    <!-- Modal/Painel de Controle -->
    <Modal :isOpen="showPanel" @close="showPanel = false">
      <div class="container text-center">
        <h3>Configurar Batimentos Binaurais</h3> <br>
        
        <!-- Seção de Presets -->
        <div class="flex flex-col ">
          <div class="">
            <button
              v-for="(preset, index) in presets"
              :key="index"
              @click="applyPreset(preset)"
              class="bg-gray-700 m-2 rounded-lg"
            >
             
              <div class="p-2 text-sm">
                <div class="mb-3">
                  <span class="preset-icon">{{ preset.icon }}</span><br>
                <strong>{{ preset.name }}</strong>
                </div>
                <p class="mt-1">{{ preset.desc }}</p>
                <span class="preset-freq">
                  {{ preset.base }}Hz ±{{ preset.beat }}Hz
                </span>
              </div>
            </button>
          </div>
        </div>

        <!-- Controles Manuais -->
        
          <div class="flex justify-center mt-4 text-sm">
          <div class="">
            <label class="text-gray-300">Frequência Base: {{ baseFreq }}Hz</label>
            <input
              type="range"
              v-model="baseFreq"
              min="100"
              max="500"
              step="1"
              class="mt-1 w-24"
            >
          </div>
          
          <div class="ml-2">
            <label class="text-gray-300">Diferença: {{ beatFreq }}Hz</label>
            <input
              type="range"
              v-model="beatFreq"
              min="1"
              max="30"
              step="0.1"
              class="mt-1 w-24"
            >
          </div>
        </div>
      
       

       
        <div class="mt-4">
          <button
            v-if="!binauralActive"
            @click="activateBinaural"
            class="bg-primary hover:bg-primary/90 px-2 py-2.5 rounded-lg font-medium text-sm transition-all"
          >
            Ativar Batimentos
          </button>
          <button
            v-else
            @click="deactivateBinaural"
            class="bg-red-400 px-2 py-2.5 rounded-lg font-medium text-sm transition-all"
          >
            Desativar
          </button>
        </div>
      </div>
    </Modal>
  </div>
</template>

