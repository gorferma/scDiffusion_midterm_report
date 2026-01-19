<script setup lang="ts">
import { ref, computed } from 'vue'

const dogs = [
  { name: 'Golden Retriever', trait: 'Friendly', energy: 90, color: '#FCD34D' },
  { name: 'Husky', trait: 'Dramatic', energy: 100, color: '#94A3B8' },
  { name: 'Pug', trait: 'Sleepy', energy: 20, color: '#FCA5A5' },
  { name: 'Border Collie', trait: 'Genius', energy: 95, color: '#10B981' }
]

const currentIndex = ref(0)
const currentDog = computed(() => dogs[currentIndex.value])

function nextDog() {
  currentIndex.value = (currentIndex.value + 1) % dogs.length
}
</script>

<template>
  <div class="p-6 border border-gray-400/30 rounded-xl bg-black/20 backdrop-blur-md flex gap-8 items-center">
    
    <!-- Dynamic SVG Dog Representation -->
    <div class="relative w-40 h-40 flex-shrink-0 transition-all duration-500 ease-out transform hover:scale-110">
      <svg viewBox="0 0 100 100" class="w-full h-full drop-shadow-lg">
        <!-- Head -->
        <circle cx="50" cy="50" r="40" :fill="currentDog.color" class="transition-colors duration-500" />
        <!-- Ears (move slightly based on index) -->
        <path d="M 20 20 L 30 50 L 10 40 Z" :fill="currentDog.color" class="transition-all duration-500" :transform="`rotate(${currentIndex * 10 - 15} 20 20)`" />
        <path d="M 80 20 L 70 50 L 90 40 Z" :fill="currentDog.color" class="transition-all duration-500" :transform="`rotate(${-(currentIndex * 10 - 15)} 80 20)`" />
        <!-- Eyes -->
        <circle cx="35" cy="45" r="5" fill="black" />
        <circle cx="65" cy="45" r="5" fill="black" />
        <!-- Snout -->
        <ellipse cx="50" cy="65" rx="12" ry="8" fill="rgba(0,0,0,0.2)" />
        <circle cx="50" cy="60" r="3" fill="black" />
      </svg>
    </div>

    <!-- Info Card -->
    <div class="flex-1">
      <div class="flex justify-between items-center mb-2">
        <h2 class="text-2xl font-bold m-0" :style="{ color: currentDog.color }">{{ currentDog.name }}</h2>
        <span class="text-xs px-2 py-1 bg-white/10 rounded-full">{{ currentDog.trait }}</span>
      </div>
      
      <div class="space-y-2">
        <div class="flex items-center gap-2 text-sm text-gray-400">
          <span>Energy Level</span>
        </div>
        <div class="h-2 w-full bg-gray-700 rounded-full overflow-hidden">
          <div 
            class="h-full bg-gradient-to-r from-teal-400 to-blue-500 transition-all duration-700 ease-out"
            :style="{ width: `${currentDog.energy}%` }"
          ></div>
        </div>
      </div>

      <button 
        @click="nextDog"
        class="mt-6 px-4 py-2 text-sm rounded bg-white/10 hover:bg-white/20 hover:scale-105 active:scale-95 transition-all w-full"
      >
        Next Good Boy 🦴
      </button>
    </div>

  </div>
</template>
