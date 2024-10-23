```vue
<template>
  <div class="cinematic-background">
    <!-- Ambient Light Effects -->
    <div class="spotlight spotlight-1"></div>
    <div class="spotlight spotlight-2"></div>
    <div class="spotlight spotlight-3"></div>
    
    <!-- Film Reel Animation -->
    <div class="film-reel-container">
      <div v-for="n in 20" :key="`reel-${n}`" class="film-frame"></div>
    </div>
    
    <!-- Floating Movie Elements -->
    <div class="floating-elements">
      <div v-for="n in 15" :key="`element-${n}`" 
           class="floating-element"
           :style="getFloatingStyle(n)">
        {{ getMovieSymbol(n) }}
      </div>
    </div>

    <!-- Gradient Overlay -->
    <div class="gradient-overlay"></div>
    
    <!-- Animated Particles -->
    <div class="particles">
      <div v-for="n in 50" :key="`particle-${n}`" 
           class="particle"
           :style="getParticleStyle()">
      </div>
    </div>

    <!-- Cinematic Lines Effect -->
    <div class="cinematic-lines"></div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted } from 'vue'

const movieSymbols = ['🎬', '🎥', '🎭', '🎪', '🎟️', '🍿', '📽️', '🎦']

const getFloatingStyle = (index) => {
  const delay = (index * 0.5) % 4
  const duration = 6 + Math.random() * 4
  const size = 24 + Math.random() * 24
  
  return {
    left: `${Math.random() * 100}%`,
    top: `${Math.random() * 100}%`,
    animationDelay: `${delay}s`,
    animationDuration: `${duration}s`,
    fontSize: `${size}px`,
    opacity: 0.4 + Math.random() * 0.3
  }
}

const getParticleStyle = () => {
  return {
    left: `${Math.random() * 100}%`,
    top: `${Math.random() * 100}%`,
    animationDelay: `${Math.random() * 5}s`,
    animationDuration: `${3 + Math.random() * 7}s`
  }
}

const getMovieSymbol = (index) => {
  return movieSymbols[index % movieSymbols.length]
}

let interval
onMounted(() => {
  interval = setInterval(() => {
    const lines = document.querySelector('.cinematic-lines')
    if (lines) {
      lines.style.opacity = Math.random() * 0.3
    }
  }, 100)
})

onUnmounted(() => {
  clearInterval(interval)
})
</script>

<style scoped>
.cinematic-background {
  position: fixed;
  inset: 0;
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  overflow: hidden;
}

/* Ambient Light Effects */
.spotlight {
  position: absolute;
  width: 60vw;
  height: 60vw;
  border-radius: 50%;
  filter: blur(100px);
  opacity: 0.15;
  animation: moveSpotlight 10s infinite ease-in-out;
}

.spotlight-1 {
  background: #ff4b4b;
  top: -20%;
  left: -20%;
  animation-delay: 0s;
}

.spotlight-2 {
  background: #4b83ff;
  bottom: -20%;
  right: -20%;
  animation-delay: -3.33s;
}

.spotlight-3 {
  background: #ffd700;
  top: 50%;
  left: 50%;
  animation-delay: -6.66s;
}

/* Film Reel Animation */
.film-reel-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100px;
  display: flex;
  gap: 2px;
  animation: slideReel 20s linear infinite;
}

.film-frame {
  flex-shrink: 0;
  width: 40px;
  height: 100%;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

/* Floating Elements */
.floating-elements {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.floating-element {
  position: absolute;
  animation: float 8s ease-in-out infinite;
}

/* Gradient Overlay */
.gradient-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    circle at 50% 50%,
    transparent 0%,
    rgba(26, 26, 46, 0.5) 100%
  );
}

/* Particles */
.particles {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.particle {
  position: absolute;
  width: 2px;
  height: 2px;
  background: white;
  border-radius: 50%;
  animation: particleFloat 5s ease-in-out infinite;
}

/* Cinematic Lines Effect */
.cinematic-lines {
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 1px,
    rgba(255, 255, 255, 0.1) 1px,
    rgba(255, 255, 255, 0.1) 2px
  );
  opacity: 0.1;
  pointer-events: none;
}

@keyframes moveSpotlight {
  0%, 100% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(100px, 100px);
  }
}

@keyframes slideReel {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-50%);
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  50% {
    transform: translateY(-20px) rotate(10deg);
  }
}

@keyframes particleFloat {
  0%, 100% {
    transform: translateY(0) translateX(0);
    opacity: 0;
  }
  25% {
    opacity: 1;
  }
  50% {
    transform: translateY(-100px) translateX(50px);
    opacity: 0.5;
  }
  75% {
    opacity: 1;
  }
}
</style>
```