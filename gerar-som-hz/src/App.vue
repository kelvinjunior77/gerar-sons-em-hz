<script setup>
import { ref, onMounted, watch } from 'vue'
import FrequencyInput from './components/FrequencyInput.vue'
import DurationInput from './components/DurationInput.vue'
import ToneControls from './components/ToneControls.vue'
import StatusDisplay from './components/StatusDisplay.vue'
import WaveTypeSelector from './components/WaveTypeSelector.vue'
import VolumeControl from './components/VolumeControl.vue'
import FrequencyPresets from './components/FrequencyPresets.vue'
import AudioVisualizer from './components/AudioVisualizer.vue'
import BinauralBeats from './components/BinauralBeats.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'
import Footer from './components/Footer.vue'

// salvar configurações
const saveSettings = () => {
  localStorage.setItem('audioSettings', JSON.stringify({
    volume: volume.value,
    waveType: waveType.value,
    frequency: frequency.value
  }))
}

// E carregar no onMounted
const loadSettings = () => {
  const saved = JSON.parse(localStorage.getItem('audioSettings'))
  if (saved) {
    volume.value = saved.volume
    waveType.value = saved.waveType
    frequency.value = saved.frequency
  }
}

// Estados de loading
const isLoading = ref(true)
const loadingMessage = ref("Inicializando sistema de áudio...")
const loadingProgress = ref(0)


onMounted(async () => {
  try {
    // Tempo mínimo de loading (melhor experiência)
    const minLoadingTime = new Promise(resolve => setTimeout(resolve, 1000))

    // Inicialização real
    await initAudioContext()

    // Garante o tempo mínimo
    await minLoadingTime
  } catch (e) {
    console.error("Erro na inicialização:", e)
    error.value = "Erro ao inicializar o sistema de áudio"
  } finally {
    isLoading.value = false
  }
})

const updateLoading = (message, progress) => {
  loadingMessage.value = message
  loadingProgress.value = progress
}


const initAudioContext = async () => {
  try {
    loadingMessage.value = "Criando contexto de áudio..."
    audioContext = new (window.AudioContext || window.webkitAudioContext)()

    loadingMessage.value = "Configurando analisador..."
    setupAnalyser()

    loadingMessage.value = "Preparando recursos..."
    // Adicione aqui qualquer outra inicialização necessária
  } catch (e) {
    throw e
  }
}



// gravacao de audio
const isRecording = ref(false)
const audioBlob = ref(null)
const audioUrl = ref('')
let mediaRecorder = null
let audioChunks = ref([])

// Estado reativo
const frequency = ref(528)
const duration = ref(2)
const isPlaying = ref(false)
const infinitePlay = ref(false)
const error = ref(null)
const waveType = ref('sine')
const volume = ref(0.5)
const useBinaural = ref(false)
const baseFrequency = ref(200)
const beatFrequency = ref(10)

// Referências para a API de Áudio
let audioContext = null
let oscillator = null
let oscillator2 = null
let gainNode = null
let gainNode2 = null
let analyser = null
let stopTimeout = null

// Inicialização do AudioContext
onMounted(() => {
  try {
    audioContext = new (window.AudioContext || window.webkitAudioContext)()
    setupAnalyser()
  } catch (e) {
    error.value = "Seu navegador não suporta a Web Audio API"
  }
})

// Configura o analisador para visualização
const setupAnalyser = () => {
  analyser = audioContext.createAnalyser()
  analyser.fftSize = 2048
}

// Função para inicializar os nós de áudio
const initAudioNodes = () => {
  // Configuração do oscilador principal
  oscillator = audioContext.createOscillator()
  gainNode = audioContext.createGain()

  oscillator.type = waveType.value
  oscillator.frequency.setValueAtTime(frequency.value, audioContext.currentTime)

  gainNode.gain.setValueAtTime(0, audioContext.currentTime)
  gainNode.gain.linearRampToValueAtTime(volume.value, audioContext.currentTime + 0.1)

  // Conexão para visualização
  oscillator.connect(gainNode)
  gainNode.connect(analyser)
  analyser.connect(audioContext.destination)

  // Configuração do segundo oscilador para batimentos binaurais
  if (useBinaural.value) {
    oscillator2 = audioContext.createOscillator()
    gainNode2 = audioContext.createGain()

    oscillator2.type = waveType.value
    const freq2 = baseFrequency.value + beatFrequency.value
    oscillator2.frequency.setValueAtTime(freq2, audioContext.currentTime)

    gainNode2.gain.setValueAtTime(0, audioContext.currentTime)
    gainNode2.gain.linearRampToValueAtTime(volume.value, audioContext.currentTime + 0.1)

    oscillator2.connect(gainNode2)
    gainNode2.connect(analyser)

    oscillator2.start()
  }

  oscillator.start()
  isPlaying.value = true

  // Cria um MediaStreamDestination para gravação
  const destination = audioContext.createMediaStreamDestination()
  gainNode.connect(destination)
  if (useBinaural.value) {
    gainNode2.connect(destination)
  }

  // Configura o MediaRecorder
  mediaRecorder = new MediaRecorder(destination.stream)
  audioChunks.value = []

  mediaRecorder.ondataavailable = (evt) => {
    audioChunks.value.push(evt.data)
  }

  mediaRecorder.onstop = () => {
    const blob = new Blob(audioChunks.value, { type: 'audio/wav' })
    audioBlob.value = blob
    audioUrl.value = URL.createObjectURL(blob)
  }

  if (isRecording.value) {
    mediaRecorder.start()
  }

}

// Adicione estas novas funções
const toggleRecording = () => {
  if (!isPlaying.value) {
    error.value = "Você precisa estar reproduzindo para gravar"
    return
  }

  if (isRecording.value) {
    mediaRecorder.stop()
    isRecording.value = false
  } else {
    isRecording.value = true
    audioChunks.value = []
    mediaRecorder.start()
  }
}

const downloadAudio = () => {
  if (!audioBlob.value) return

  const a = document.createElement('a')
  a.href = audioUrl.value
  a.download = `soundhz${frequency.value}hz_${new Date().toISOString().slice(0, 10)}.wav`
  a.click()
}

// Funções de controle
const playTone = () => {
  if (!audioContext || isPlaying.value) return

  error.value = null
  infinitePlay.value = false

  if (frequency.value < 20 || frequency.value > 20000) {
    error.value = "Por favor, insira uma frequência entre 20Hz e 20000Hz"
    return
  }

  try {
    initAudioNodes()

    stopTimeout = setTimeout(() => {
      if (isPlaying.value && !infinitePlay.value) {
        stopTone()
      }
    }, duration.value * 1000)
  } catch (e) {
    error.value = "Erro ao reproduzir o som: " + e.message
  }
}

const toggleInfinitePlay = () => {
  infinitePlay.value ? stopTone() : startInfinitePlay()
}

const startInfinitePlay = () => {
  if (!audioContext || isPlaying.value) return

  error.value = null
  infinitePlay.value = true

  if (frequency.value < 20 || frequency.value > 20000) {
    error.value = "Por favor, insira uma frequência entre 20Hz e 20000Hz"
    infinitePlay.value = false
    return
  }

  try {
    initAudioNodes()
  } catch (e) {
    error.value = "Erro ao reproduzir o som: " + e.message
    infinitePlay.value = false
  }
}

const stopTone = () => {
  if ((!oscillator && !oscillator2) || !isPlaying.value) return

  clearTimeout(stopTimeout)
  stopTimeout = null

  try {
    const fadeOutTime = 0.1
    const now = audioContext.currentTime
    gainNode.gain.linearRampToValueAtTime(0, now + fadeOutTime)

    if (gainNode2) {
      gainNode2.gain.linearRampToValueAtTime(0, now + fadeOutTime)
    }

    setTimeout(() => {
      oscillator?.stop()
      oscillator?.disconnect()
      oscillator2?.stop()
      oscillator2?.disconnect()
      gainNode?.disconnect()
      gainNode2?.disconnect()
      isPlaying.value = false
      infinitePlay.value = false
    }, fadeOutTime * 1000)
  } catch (e) {
    error.value = "Erro ao parar o som: " + e.message
  }
}

// Atualiza o tipo de onda durante a reprodução
watch(waveType, (newVal) => {
  if (!isPlaying.value) return
  oscillator.type = newVal
  if (oscillator2) oscillator2.type = newVal
})

// Atualiza o volume durante a reprodução
watch(volume, (newVal) => {
  if (!isPlaying.value) return
  gainNode.gain.linearRampToValueAtTime(newVal, audioContext.currentTime + 0.1)
  if (gainNode2) gainNode2.gain.linearRampToValueAtTime(newVal, audioContext.currentTime + 0.1)
})

// Atualiza a frequência durante a reprodução
watch(frequency, (newVal) => {
  if (!isPlaying.value) return
  oscillator.frequency.setValueAtTime(newVal, audioContext.currentTime)
})

// Atualiza batimentos binaurais
watch([baseFrequency, beatFrequency], () => {
  if (!isPlaying.value || !useBinaural.value) return
  oscillator.frequency.setValueAtTime(baseFrequency.value, audioContext.currentTime)
  oscillator2.frequency.setValueAtTime(baseFrequency.value + beatFrequency.value, audioContext.currentTime)
})
</script>

<template>

  <LoadingSpinner v-if="isLoading" :message="loadingMessage" :progress="loadingProgress" />

  <!----<div v-if="isLoading" class="loading-overlay">
    <div class="loading-container">
      <div class="loading-spinner"></div>
      <p>{{ loadingMessage }}</p>
    </div>
  </div> ------>

  <div v-else class="bg-gradient-to-br from-dark to-gray-900 min-h-screen font-sans text-light">
    <!-- Header/Navbar -->
    <header class="py-6 px-4 sm:px-6 lg:px-8">
      <div class="container mx-auto flex justify-between items-center">
        <div class="flex items-center space-x-2">
          <svg class="w-8 h-8 text-secondary" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3">
            </path>
          </svg>
          <h1 class="text-2xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-primary to-secondary">
            SoundHz
          </h1>
        </div>
        <!----
        <nav class="hidden md:flex space-x-8">
          <a href="#" class="hover:text-primary transition-colors">Início</a>
          <a href="#" class="hover:text-primary transition-colors">Recursos</a>
          <a href="#" class="hover:text-primary transition-colors">Sobre</a>
          <a href="#" class="hover:text-primary transition-colors">Contato</a>
        </nav>
        <button class="md:hidden text-gray-300">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
          </svg>
        </button> 
      ---->
      </div>
    </header>


    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-12">
      <div class="max-w-4xl mx-auto">

        <section class="text-center mb-16">
          <h2 class="text-4xl md:text-5xl font-bold mb-6 leading-tight">
            Crie <span class="bg-clip-text text-transparent bg-gradient-to-r from-primary to-secondary">sons
              precisos</span> para suas necessidades
          </h2>
          <p class="text-lg text-gray-300 max-w-2xl mx-auto mb-8">
            Gere frequências específicas, batimentos binaurais e explore o poder do som
          </p>
          <div class="flex justify-center space-x-4">
            <a href="#comecar"
              class="bg-primary hover:bg-primary/90 px-6 py-3 rounded-lg font-medium transition-all transform hover:scale-105">
              Começar Agora
            </a>

            <a href="https://github.com/kelvinjunior77/gerar-sons-em-hz" class="border border-primary text-primary hover:bg-primary/10 px-6 py-3 rounded-lg font-medium transition-all">
            Repositório
            </a>

          </div>
        </section>

        <section id="comecar" class="bg-gray-800/50 backdrop-blur-sm rounded-xl p-6 shadow-xl border border-gray-700/50">
         

          <div class="space-y-6">
            <FrequencyInput v-model="frequency" :disabled="isPlaying" />
            <DurationInput v-model="duration" :disabled="infinitePlay || isPlaying" />
            <FrequencyPresets v-model="frequency" :disabled="isPlaying" class="presets" />
            <WaveTypeSelector v-model="waveType" :disabled="isPlaying" />
            <VolumeControl v-model="volume" />
            <AudioVisualizer v-if="isPlaying && analyser" :analyser="analyser" class="visualizer" />

            <ToneControls :is-playing="isPlaying" :infinite-play="infinitePlay" @play="playTone" @stop="stopTone"
              @toggle-infinite="toggleInfinitePlay">

              <template #gravacao>
                <button
                  class="border border-rose-500 border-primary hover:bg-primary/10 px-2 py-2.5 rounded-lg text-rose-500 font-medium text-sm  flex items-center transition-all cursor-pointer"
                  @click="toggleRecording" :class="{ 'btn-recording': isRecording }" :disabled="!isPlaying">
                  <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z">
                    </path>
                  </svg>
                  {{ isRecording ? 'Parar Gravação' : 'Gravar' }}
                </button>

                <button @click="downloadAudio"
                  class="bg-secondary hover:bg-secondary/90 px-2 py-2.5 rounded-lg font-medium text-sm  flex items-center transition-all ml-auto cursor-pointer"
                  :disabled="!audioBlob">
                  <svg class="w-5 h-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path>
                  </svg>
                  Exportar Áudio
                </button>
              </template>

            </ToneControls>
         
              <BinauralBeats v-model:enabled="useBinaural" v-model:base="baseFrequency" v-model:beat="beatFrequency"
                :disabled="isPlaying" />

                <StatusDisplay :is-playing="isPlaying" :infinite-play="infinitePlay" :frequency="frequency" :duration="duration"
          :error="error" :wave-type="waveType" :volume="volume" :binaural-enabled="useBinaural"
          :beat-frequency="beatFrequency" />
       

          </div>
        </section>


      </div>
    </main>


     <!-- Footer -->
    <Footer />

  </div>

</template>

<style>
.wave-visualizer {
  background: linear-gradient(90deg, transparent, rgba(99, 102, 241, 0.2), transparent);
  animation: wave 1.5s linear infinite;
  opacity: 0.8;
}

@keyframes wave {
  0% {
    transform: translateX(-100%);
  }

  100% {
    transform: translateX(100%);
  }
}
</style>