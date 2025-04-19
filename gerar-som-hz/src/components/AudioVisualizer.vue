<script setup>
import { onMounted, watch, ref } from 'vue'

const props = defineProps({
  analyser: {
    type: Object,
    required: true
  }
})

const canvasRef = ref(null)
let animationId = null
let canvasCtx = null

onMounted(() => {
  canvasCtx = canvasRef.value.getContext('2d')
  draw()
})

const draw = () => {
  animationId = requestAnimationFrame(draw)
  
  const bufferLength = props.analyser.frequencyBinCount
  const dataArray = new Uint8Array(bufferLength)
  props.analyser.getByteTimeDomainData(dataArray)
  
  canvasCtx.fillStyle = 'rgb(200, 200, 200)'
  canvasCtx.fillRect(0, 0, canvasRef.value.width, canvasRef.value.height)
  
  canvasCtx.lineWidth = 2
  canvasCtx.strokeStyle = 'rgb(0, 0, 0)'
  canvasCtx.beginPath()
  
  const sliceWidth = canvasRef.value.width / bufferLength
  let x = 0

  for (let i = 0; i < bufferLength; i++) {
    const v = dataArray[i] / 128.0
    const y = v * canvasRef.value.height / 2

    if (i === 0) {
      canvasCtx.moveTo(x, y)
    } else {
      canvasCtx.lineTo(x, y)
    }

    x += sliceWidth
  }

  canvasCtx.lineTo(canvasRef.value.width, canvasRef.value.height / 2)
  canvasCtx.stroke()
}

watch(() => props.analyser, () => {
  if (animationId) {
    cancelAnimationFrame(animationId)
  }
  draw()
})
</script>

<template>
  <canvas ref="canvasRef" width="800" height="150"></canvas>
</template>