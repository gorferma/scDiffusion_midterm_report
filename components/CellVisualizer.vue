<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Dot {
  x: number
  y: number
  r: number
  color: string
  targetX: number
  targetY: number
}

const dots = ref<Dot[]>([])
const isRunning = ref(false)
const progress = ref(0) // 0 to 1
let animationFrameId: number | null = null

const clusters = [
  { x: 25, y: 25, color: '#4EC5D4' }, // Teal
  { x: 75, y: 25, color: '#F06292' }, // Pink
  { x: 50, y: 75, color: '#FFD54F' }  // Yellow
]

function generateNoise() {
  const newDots: Dot[] = []
  for (let i = 0; i < 60; i++) {
    // Assign a random cluster to belong to
    const cluster = clusters[Math.floor(Math.random() * clusters.length)]
    
    // Start at completely random position (Pure Noise)
    const startX = Math.random() * 100
    const startY = Math.random() * 100
    
    // Target is the cluster center + some natural spread (Gaussian-ish)
    // We use a simple approximation for spread
    const spread = 10
    const targetX = cluster.x + (Math.random() - 0.5) * spread
    const targetY = cluster.y + (Math.random() - 0.5) * spread

    newDots.push({
      x: startX,
      y: startY,
      r: Math.random() * 2 + 1.5,
      color: cluster.color, // In a real model, we wouldn't know the class yet, but for viz we show it
      targetX,
      targetY
    })
  }
  dots.value = newDots
  progress.value = 0
  isRunning.value = false
  if (animationFrameId) cancelAnimationFrame(animationFrameId)
}

function toggleProcess() {
  if (isRunning.value) {
    // Reset
    generateNoise()
  } else {
    // Start Denoising
    isRunning.value = true
    animate()
  }
}

function animate() {
  if (!isRunning.value) return
  
  // Speed of convergence
  const speed = 0.005
  progress.value += speed

  if (progress.value >= 1) {
    progress.value = 1
    isRunning.value = false
    return
  }

  // Noise decreases as we progress (Reverse Diffusion)
  // At progress 0, noise is high. At progress 1, noise is 0.
  const noiseMagnitude = (1 - progress.value) * 2.0 

  dots.value = dots.value.map(d => {
    // Linear interpolation towards target
    // We move a fraction of the remaining distance
    const moveFactor = 0.05
    const dx = d.targetX - d.x
    const dy = d.targetY - d.y
    
    return {
      ...d,
      x: d.x + dx * moveFactor + (Math.random() - 0.5) * noiseMagnitude,
      y: d.y + dy * moveFactor + (Math.random() - 0.5) * noiseMagnitude
    }
  })
  
  animationFrameId = requestAnimationFrame(animate)
}

onMounted(() => {
  generateNoise()
})
</script>

<template>
  <div class="p-4 border border-gray-400/30 rounded-xl bg-black/20 backdrop-blur-md">
    <div class="flex justify-between items-center mb-4">
      <h3 class="text-lg font-bold">Reverse Diffusion Process</h3>
      <div class="flex items-center gap-4">
        <div class="text-xs text-gray-300 font-mono">
          Step: {{ Math.floor(progress * 1000) }}
        </div>
        <button 
          @click="toggleProcess"
          class="px-4 py-1 rounded text-white transition font-medium min-w-[100px]"
          :class="isRunning ? 'bg-red-500 hover:bg-red-600' : (progress >= 1 ? 'bg-gray-500 hover:bg-gray-600' : 'bg-teal-600 hover:bg-teal-500')"
        >
          {{ isRunning ? 'Stop' : (progress >= 1 ? 'Reset' : 'Denoise') }}
        </button>
      </div>
    </div>
    
    <div class="relative w-full h-60 bg-gray-900/50 rounded-lg overflow-hidden border border-gray-700">
        <!-- Grid lines for scientific feel -->
        <div class="absolute inset-0 opacity-20 pointer-events-none" 
             style="background-image: linear-gradient(#333 1px, transparent 1px), linear-gradient(90deg, #333 1px, transparent 1px); background-size: 20px 20px;">
        </div>

        <svg viewBox="0 0 100 100" class="absolute inset-0 w-full h-full">
        <circle 
            v-for="(dot, i) in dots" 
            :key="i"
            :cx="dot.x" 
            :cy="dot.y" 
            :r="dot.r" 
            :fill="dot.color"
            class="transition-transform duration-75"
        />
        </svg>
    </div>
    <p class="text-xs text-gray-400 mt-2 text-center">
      Visualizing the transition from Latent Noise ($t=T$) to Data Manifold ($t=0$)
    </p>
  </div>
</template>