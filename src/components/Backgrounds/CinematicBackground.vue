```vue
<template>
  <div class="cinematic-container">
    <!-- Morphing Backdrop -->
    <div class="morphing-background">
      <div class="morph-shape shape1"></div>
      <div class="morph-shape shape2"></div>
      <div class="morph-shape shape3"></div>
    </div>

    <!-- Enhanced Light Streaks -->
    <div class="light-streaks">
      <div v-for="n in 12" :key="`streak-${n}`" 
           class="light-streak"
           :style="getStreakStyle(n)">
      </div>
    </div>

    <!-- Enhanced Mesh Grid -->
    <div class="mesh-grid">
      <div v-for="n in 150" :key="`dot-${n}`" 
           class="mesh-dot"
           :style="getDotStyle(n)">
        <div class="dot-glow"></div>
      </div>
    </div>

    <!-- Dynamic Light Rays -->
    <div class="light-rays">
      <div v-for="n in 5" :key="`ray-${n}`"
           class="light-ray"
           :style="getRayStyle(n)">
      </div>
    </div>

    <!-- Enhanced Color Overlay -->
    <div class="color-overlay"></div>

    <!-- Cinematic Vignette -->
    <div class="vignette"></div>
</div>
</template>

<script setup>
const getStreakStyle = (index) => {
  const rotation = (index * 30) % 360
  const delay = index * -1.2
  return {
    transform: `rotate(${rotation}deg)`,
    animationDelay: `${delay}s`
  }
}

const getDotStyle = (index) => {
  const row = Math.floor(index / 15)
  const col = index % 15
  const delay = (row + col) * 0.1
  return {
    left: `${col * 7}%`,
    top: `${row * 7}%`,
    animationDelay: `${delay}s`
  }
}

const getRayStyle = (index) => {
  const rotation = index * 72 // 360/5
  return {
    transform: `rotate(${rotation}deg)`,
    animationDelay: `${index * -1.5}s`
  }
}
</script>

<style scoped>
.cinematic-container {
  position: fixed;
  inset: 0;
  background: #0A0A1F;
  overflow: hidden;
  perspective: 1500px;
}

/* Morphing Background */
.morphing-background {
  position: absolute;
  inset: 0;
  filter: blur(80px);
}

.morph-shape {
  position: absolute;
  border-radius: 50%;
  animation: morphAnimation 15s infinite ease-in-out;
}

.shape1 {
  width: 100vh;
  height: 100vh;
  background: rgba(103, 95, 242, 0.4);
  top: -50vh;
  left: -20vw;
  animation-delay: 0s;
}

.shape2 {
  width: 90vh;
  height: 90vh;
  background: rgba(219, 61, 207, 0.4);
  bottom: -45vh;
  right: -15vw;
  animation-delay: -5s;
}

.shape3 {
  width: 80vh;
  height: 80vh;
  background: rgba(60, 57, 126, 0.4);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -10s;
}

/* Enhanced Light Streaks */
.light-streaks {
  position: absolute;
  inset: 0;
  transform-style: preserve-3d;
}

.light-streak {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 200%;
  height: 3px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(103, 95, 242, 0.2),
    rgba(219, 61, 207, 0.3),
    rgba(103, 95, 242, 0.2),
    transparent
  );
  transform-origin: left center;
  animation: streakMove 6s linear infinite;
}

/* Enhanced Mesh Grid */
.mesh-grid {
  position: absolute;
  inset: 0;
  perspective: 1500px;
  transform: rotateX(60deg) scale(1.5);
  opacity: 0.7;
}

.mesh-dot {
  position: absolute;
  width: 6px;
  height: 6px;
  background: #675FF2;
  border-radius: 50%;
  animation: pulseDot 4s infinite ease-in-out;
}

.dot-glow {
  position: absolute;
  inset: -2px;
  background: radial-gradient(
    circle at center,
    rgba(103, 95, 242, 0.4),
    transparent 70%
  );
  border-radius: 50%;
}

/* Dynamic Light Rays */
.light-rays {
  position: absolute;
  inset: 0;
  transform-style: preserve-3d;
}

.light-ray {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 200%;
  height: 150px;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(103, 95, 242, 0.1),
    rgba(219, 61, 207, 0.15),
    transparent
  );
  transform-origin: left center;
  animation: rayRotate 15s linear infinite;
}

/* Color Overlay */
.color-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    circle at center,
    transparent 30%,
    rgba(10, 10, 31, 0.4)
  );
  mix-blend-mode: overlay;
}

/* Enhanced Vignette */
.vignette {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    circle at center,
    transparent 30%,
    rgba(10, 10, 31, 0.8) 100%
  );
  pointer-events: none;
}

@keyframes morphAnimation {
  0%, 100% {
    border-radius: 60% 40% 30% 70% / 60% 30% 70% 40%;
    transform: rotate(0deg) scale(1);
  }
  25% {
    border-radius: 30% 60% 70% 40% / 50% 60% 30% 60%;
    transform: rotate(90deg) scale(1.2);
  }
  50% {
    border-radius: 40% 60% 30% 70% / 70% 30% 50% 40%;
    transform: rotate(180deg) scale(1);
  }
  75% {
    border-radius: 60% 40% 70% 30% / 30% 70% 40% 60%;
    transform: rotate(270deg) scale(1.2);
  }
}

@keyframes streakMove {
  0% {
    transform: translateX(-100%) rotate(var(--rotation));
    opacity: 0;
  }
  10% {
    opacity: 1;
  }
  90% {
    opacity: 1;
  }
  100% {
    transform: translateX(100%) rotate(var(--rotation));
    opacity: 0;
  }
}

@keyframes pulseDot {
  0%, 100% {
    transform: scale(1) translateZ(0);
    opacity: 0.6;
  }
  50% {
    transform: scale(1.8) translateZ(30px);
    opacity: 1;
  }
}

@keyframes rayRotate {
  0% {
    transform: rotate(var(--rotation)) translateX(-50%) scale(0.5);
    opacity: 0;
  }
  20% {
    opacity: 1;
  }
  80% {
    opacity: 1;
  }
  100% {
    transform: rotate(calc(var(--rotation) + 360deg)) translateX(-50%) scale(0.5);
    opacity: 0;
  }
}

/* Add smooth motion reduction for users who prefer reduced motion */
@media (prefers-reduced-motion: reduce) {
  .morph-shape,
  .light-streak,
  .mesh-dot,
  .light-ray {
    animation: none;
  }
}
</style>
```