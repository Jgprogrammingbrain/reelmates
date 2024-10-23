```vue
<template>
  <div class="theater-background">
    <!-- Moving Spotlights -->
    <div class="spotlights">
      <div class="spotlight spot-1"></div>
      <div class="spotlight spot-2"></div>
      <div class="spotlight spot-3"></div>
    </div>

    <!-- Animated Star Field -->
    <div class="star-field">
      <div v-for="n in 100" :key="`star-${n}`" 
           class="star"
           :style="getStarStyle()">
      </div>
    </div>

    <!-- Theater Curtains -->
    <div class="curtains">
      <div class="curtain left"></div>
      <div class="curtain right"></div>
    </div>

    <!-- Neon Grid Lines -->
    <div class="neon-grid">
      <div v-for="n in 10" :key="`line-${n}`" 
           class="grid-line"
           :style="{ animationDelay: `${n * 0.2}s` }">
      </div>
    </div>
  </div>
</template>

<script setup>
const getStarStyle = () => ({
  left: `${Math.random() * 100}%`,
  top: `${Math.random() * 100}%`,
  animationDelay: `${Math.random() * 5}s`,
  animationDuration: `${2 + Math.random() * 3}s`
})
</script>

<style scoped>
.theater-background {
  position: fixed;
  inset: 0;
  background: #000000;
  overflow: hidden;
  perspective: 1000px;
}

.spotlights {
  position: absolute;
  inset: 0;
  filter: blur(100px);
}

.spotlight {
  position: absolute;
  width: 60vh;
  height: 60vh;
  border-radius: 50%;
  opacity: 0.15;
  animation: moveSpotlight 15s infinite ease-in-out;
}

.spot-1 {
  background: #675FF2;
  top: -30%;
  left: -10%;
  animation-delay: 0s;
}

.spot-2 {
  background: #DB3DCF;
  bottom: -30%;
  right: -10%;
  animation-delay: -5s;
}

.spot-3 {
  background: #3C397E;
  top: 50%;
  left: 50%;
  animation-delay: -10s;
}

.star-field {
  position: absolute;
  inset: 0;
}

.star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: #D0CCE3;
  border-radius: 50%;
  animation: twinkle 3s infinite ease-in-out;
}

.curtains {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.curtain {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 50%;
  background: linear-gradient(
    to bottom,
    rgba(60, 57, 126, 0.2) 0%,
    rgba(60, 57, 126, 0.1) 100%
  );
}

.curtain.left {
  left: 0;
  transform-origin: left;
  animation: curtainWave 10s infinite ease-in-out;
}

.curtain.right {
  right: 0;
  transform-origin: right;
  animation: curtainWave 10s infinite ease-in-out reverse;
}

.neon-grid {
  position: absolute;
  inset: 0;
  transform: perspective(1000px) rotateX(60deg);
  opacity: 0.3;
}

.grid-line {
  position: absolute;
  width: 200%;
  height: 1px;
  background: linear-gradient(90deg,
    transparent,
    #675FF2,
    #DB3DCF,
    transparent
  );
  animation: moveGrid 10s linear infinite;
}

@keyframes moveSpotlight {
  0%, 100% { transform: translate(0, 0); }
  50% { transform: translate(20%, 20%); }
}

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.5); }
}

@keyframes curtainWave {
  0%, 100% { transform: skewY(0deg); }
  50% { transform: skewY(1deg); }
}

@keyframes moveGrid {
  0% { transform: translateY(-100%); }
  100% { transform: translateY(100%); }
}
</style>
```