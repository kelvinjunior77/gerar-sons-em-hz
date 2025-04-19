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

// Exemplo simples para salvar configurações (adicione no App.vue)
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


// gravacao de audio
const isRecording = ref(false)
const audioBlob = ref(null)
const audioUrl = ref('')
let mediaRecorder = null
let audioChunks = ref([])

// Estado reativo
const frequency = ref(440)
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
  a.download = `tonal_${frequency.value}hz_${new Date().toISOString().slice(0,10)}.wav`
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
  <div class="container">
    <h1>Gerador de Tons Avançado</h1>
    
    <div class="settings-grid">
      <div class="input-group">
        <FrequencyInput v-model="frequency" :disabled="isPlaying" />
        <DurationInput v-model="duration" :disabled="infinitePlay || isPlaying" />
      </div>
      
      <div class="control-group">
        <WaveTypeSelector v-model="waveType" :disabled="isPlaying" />
        <VolumeControl v-model="volume" />
      </div>

      <div class="recording-controls">
    <button 
      @click="toggleRecording" 
      :class="{ 'btn-recording': isRecording }"
      :disabled="!isPlaying"
    >
      {{ isRecording ? 'Parar Gravação' : 'Iniciar Gravação' }}
    </button>
    
    <button 
      @click="downloadAudio" 
      class="btn-download"
      :disabled="!audioBlob"
    >
      Exportar Áudio
    </button>
  </div>
      
      <FrequencyPresets 
        v-model="frequency" 
        :disabled="isPlaying"
        class="presets"
      />
      
      <BinauralBeats
        v-model:enabled="useBinaural"
        v-model:base="baseFrequency"
        v-model:beat="beatFrequency"
        :disabled="isPlaying"
        class="binaural"
      />
    </div>
    
    <ToneControls
      :is-playing="isPlaying"
      :infinite-play="infinitePlay"
      @play="playTone"
      @stop="stopTone"
      @toggle-infinite="toggleInfinitePlay"
    />
    
    <AudioVisualizer 
      v-if="isPlaying && analyser" 
      :analyser="analyser"
      class="visualizer"
    />
    
    <StatusDisplay
      :is-playing="isPlaying"
      :infinite-play="infinitePlay"
      :frequency="frequency"
      :duration="duration"
      :error="error"
      :wave-type="waveType"
      :volume="volume"
      :binaural-enabled="useBinaural"
      :beat-frequency="beatFrequency"
    />
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.settings-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.input-group, .control-group {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.presets {
  grid-column: span 2;
}

.binaural {
  grid-column: span 2;
}

.visualizer {
  margin-top: 20px;
  height: 150px;
}

h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.recording-controls {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.btn-recording {
  background-color: #f44336;
  color: white;
}

.btn-download {
  background-color: #673ab7;
  color: white;
}
</style>