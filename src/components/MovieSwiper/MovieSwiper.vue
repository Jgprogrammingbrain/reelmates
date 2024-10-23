<template>
  <div class="flex flex-col items-center w-full max-w-2xl mx-auto p-4 relative">
    <SciFiBackground />
    
    <div class="card-stack">
      <MovieCard
        v-for="(movie, index) in displayedMovies"
        :key="movie.id"
        :movie="movie"
        :index="index"
        :exit-direction="exitDirection"
        @swipe="handleSwipe"
      />
    </div>

    <div class="mt-8 text-center">
      <p v-if="currentIndex >= movies.length" class="text-xl text-white">
        No more movies to swipe!
      </p>
      <p v-else class="text-xl text-white">
        {{ swipeMessage }}
      </p>
    </div>

    <div v-if="currentIndex < movies.length" class="flex gap-4 mt-4">
      <button
        @click="handleManualSwipe('left')"
        class="px-6 py-2 bg-red-500 text-white rounded-full hover:bg-red-600 transition-colors"
      >
        Pass
      </button>
      <button
        @click="handleManualSwipe('right')"
        class="px-6 py-2 bg-green-500 text-white rounded-full hover:bg-green-600 transition-colors"
      >
        Like
      </button>
    </div>

    <div v-if="likedMovies.length > 0" class="mt-8 w-full">
      <h3 class="text-2xl font-bold mb-4 text-white">Liked Movies:</h3>
      <div class="space-y-2">
        <div
          v-for="movie in likedMovies"
          :key="movie.id"
          class="bg-white bg-opacity-10 backdrop-blur-md p-3 rounded-lg flex justify-between items-center"
        >
          <span class="text-white">{{ movie.title }}</span>
          <span class="text-green-400">{{ movie.rating }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import MovieCard from './MovieCard.vue'
import SciFiBackground from './backgrounds/SciFiBackground.vue'
import { scifiMovies as movies } from '../../data/movies'

const currentIndex = ref(0)
const likedMovies = ref([])
const exitDirection = ref(null)

// Show 3 cards at a time
const displayedMovies = computed(() => {
  return movies.slice(currentIndex.value, currentIndex.value + 3)
})

const swipeMessage = computed(() => {
  return "Swipe right to like, left to pass"
})

const handleSwipe = (direction) => {
  exitDirection.value = direction
  if (direction === 'right') {
    likedMovies.value.push(movies[currentIndex.value])
  }
  
  setTimeout(() => {
    if (currentIndex.value < movies.length - 1) {
      currentIndex.value++
      exitDirection.value = null
    }
  }, 300)
}

const handleManualSwipe = (direction) => {
  handleSwipe(direction)
}
</script>

<style scoped>
.card-stack {
  position: relative;
  width: 320px;
  height: 480px;
  margin: 0 auto;
  perspective: 1000px;
}

@keyframes exitLeft {
  to {
    transform: translate(-200%, 0) rotate(-20deg);
    opacity: 0;
  }
}

@keyframes exitRight {
  to {
    transform: translate(200%, 0) rotate(20deg);
    opacity: 0;
  }
}
</style>