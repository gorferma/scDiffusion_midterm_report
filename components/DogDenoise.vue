<script setup lang="ts">
import { ref, onMounted } from 'vue'

const canvasRef = ref<HTMLCanvasElement | null>(null)
const isRunning = ref(false)
const progress = ref(0) // 0 (Noise) to 1 (Clean)
const errorMsg = ref('')
let animationFrameId: number | null = null

const imgSrc = '/Images/Synthetic_Cells.png'
const image = new Image()
let imageLoaded = false

// Store clean image data
let targetPixels: Uint8ClampedArray | null = null

function init() {
  image.src = imgSrc
  
  image.onload = () => {
    imageLoaded = true
    if (canvasRef.value) {
        const ctx = canvasRef.value.getContext('2d')
        if (ctx) {
            const internalWidth = 800
            const internalHeight = 800
            canvasRef.value.width = internalWidth
            canvasRef.value.height = internalHeight
            
            ctx.clearRect(0, 0, internalWidth, internalHeight)
            
            const scale = Math.min(internalWidth / image.width, internalHeight / image.height)
            const x = (internalWidth / 2) - (image.width / 2) * scale
            const y = (internalHeight / 2) - (image.height / 2) * scale
            
            ctx.drawImage(image, x, y, image.width * scale, image.height * scale)
            
            const imageData = ctx.getImageData(0, 0, internalWidth, internalHeight)
            const pixels = imageData.data
            
            // KEY FIX: Background Removal
            // If the image has a white background, we set its alpha to 0
            for (let i = 0; i < pixels.length; i += 4) {
              const r = pixels[i]
              const g = pixels[i+1]
              const b = pixels[i+2]
              
              // If pixel is very close to white, make it transparent
              if (r > 240 && g > 240 && b > 240) {
                pixels[i+3] = 0
              }
            }
            
            targetPixels = pixels
            draw() 
        }
    }
  }

  image.onerror = () => {
    errorMsg.value = `Error loading image: ${imgSrc}`
  }
}

function draw() {
  if (!canvasRef.value || !targetPixels) return
  const ctx = canvasRef.value.getContext('2d')
  if (!ctx) return

  const w = canvasRef.value.width
  const h = canvasRef.value.height
  
  const frameImageData = ctx.createImageData(w, h)
  const currentPixels = frameImageData.data
  
  const p = progress.value
  const invP = 1 - p
  
  for (let i = 0; i < targetPixels.length; i += 4) {
      const alpha = targetPixels[i+3]
      
      if (alpha === 0) {
        // Keep background transparent
        currentPixels[i+3] = 0
        continue
      }

      const noise = Math.random() * 255
      currentPixels[i]   = targetPixels[i]   * p + noise * invP
      currentPixels[i+1] = targetPixels[i+1] * p + noise * invP
      currentPixels[i+2] = targetPixels[i+2] * p + noise * invP
      currentPixels[i+3] = alpha
  }
  
  ctx.putImageData(frameImageData, 0, 0)
}

function handleInteraction() {
  if (isRunning.value) {
    isRunning.value = false
    if (animationFrameId) cancelAnimationFrame(animationFrameId)
  } else {
    if (progress.value >= 1) {
        progress.value = 0
        draw()
    }
    isRunning.value = true
    animate()
  }
}

function animate() {
  if (!isRunning.value) return
  progress.value += 0.006
  if (progress.value >= 1) {
    progress.value = 1
    isRunning.value = false
    draw()
    return
  }
  draw()
  animationFrameId = requestAnimationFrame(animate)
}

onMounted(() => {
  init()
})
</script>

<template>
  <div class="flex flex-col items-center cursor-pointer select-none" @click="handleInteraction">
    <div class="relative">
      <canvas 
        ref="canvasRef" 
        style="width: 320px; height: 320px;"
        class="bg-transparent"
      ></canvas>
      
      <div v-if="errorMsg" class="absolute inset-0 flex items-center justify-center bg-red-900/80 text-white text-xs p-2 text-center">
        {{ errorMsg }}
      </div>
    </div>
  </div>
</template>