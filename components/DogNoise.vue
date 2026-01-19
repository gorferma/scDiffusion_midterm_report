<template>
  <div class="flex flex-col items-center justify-center h-full">
    <canvas ref="canvas" class="max-h-100 rounded shadow cursor-pointer" @click="toggleDiffusion"></canvas>
    <div class="mt-4 flex flex-col items-center">
      <p class="text-sm font-bold text-primary mb-1">
        {{ isDiffusing ? 'Adding Noise (Diffusion Process...)' : (hasBeenDiffused ? 'Removing Noise (Reverse Diffusion...)' : 'Ready to Start') }}
      </p>
      <p class="text-xs text-gray-400">Click the image to {{ isDiffusing ? 'denoise' : 'diffuse' }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const canvas = ref(null)
const imageSrc = '/Images/dog.jpg'
const cartoonSrc = '/Images/doggo_cartoon.png'
const isDiffusing = ref(false)
const hasBeenDiffused = ref(false)

let ctx = null
let animationId = null
let originalImg = null
let cartoonImg = null

let originalData = null
let cartoonData = null
let currentImageData = null

onMounted(() => {
  const c = canvas.value
  ctx = c.getContext('2d')
  
  // Load both images
  const p1 = new Promise(resolve => {
    originalImg = new Image()
    originalImg.src = imageSrc
    originalImg.onload = resolve
  })
  
  const p2 = new Promise(resolve => {
    cartoonImg = new Image()
    cartoonImg.src = cartoonSrc
    cartoonImg.onload = resolve
  })

  Promise.all([p1, p2]).then(() => {
    setupCanvas()
  })
})

function setupCanvas() {
  const c = canvas.value
  const maxHeight = 500
  let width = originalImg.width
  let height = originalImg.height
  
  if (height > maxHeight) {
    const ratio = maxHeight / height
    height = maxHeight
    width = width * ratio
  }
  
  c.width = width
  c.height = height
  
  // Draw original and get data
  ctx.drawImage(originalImg, 0, 0, width, height)
  originalData = new Uint8ClampedArray(ctx.getImageData(0, 0, width, height).data)
  
  // Draw cartoon (off-screen/temporarily) and get data
  ctx.clearRect(0, 0, width, height)
  ctx.drawImage(cartoonImg, 0, 0, width, height)
  cartoonData = new Uint8ClampedArray(ctx.getImageData(0, 0, width, height).data)
  
  // Start with original
  ctx.clearRect(0, 0, width, height)
  ctx.drawImage(originalImg, 0, 0, width, height)
  currentImageData = ctx.getImageData(0, 0, width, height)
}

onUnmounted(() => {
  if (animationId) cancelAnimationFrame(animationId)
})

const toggleDiffusion = () => {
  isDiffusing.value = !isDiffusing.value
  if (isDiffusing.value) hasBeenDiffused.value = true
  
  if (!animationId) {
    animate()
  }
}

const animate = () => {
  if (!ctx || !originalData || !cartoonData) return

  const w = canvas.value.width
  const h = canvas.value.height
  const data = currentImageData.data
  const totalPixels = w * h
  
  const rate = 0.03 
  const pixelsToChange = Math.floor(totalPixels * rate) 
  
  for (let i = 0; i < pixelsToChange; i++) {
    const x = Math.floor(Math.random() * w)
    const y = Math.floor(Math.random() * h)
    const index = (y * w + x) * 4
    
    if (isDiffusing.value) {
      // DIFFUSE: Add noise
      const noise = Math.random() * 255
      data[index] = noise
      data[index + 1] = noise
      data[index + 2] = noise
    } else {
      // REVERSE: Turn noise into CARTOON dog
      data[index] = cartoonData[index]
      data[index + 1] = cartoonData[index + 1]
      data[index + 2] = cartoonData[index + 2]
    }
  }
  
  ctx.putImageData(currentImageData, 0, 0)
  animationId = requestAnimationFrame(animate)
}
</script>
