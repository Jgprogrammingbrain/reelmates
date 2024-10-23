```vue
<template>
  <div class="modern-background">
    <!-- Animated Gradient Orbs -->
    <div class="orbs-container">
      <div class="orb orb-1"></div>
      <div class="orb orb-2"></div>
      <div class="orb orb-3"></div>
    </div>

    <!-- Animated Grid Lines -->
    <div class="grid-lines">
      <div v-for="n in 20" :key="`line-${n}`" class="grid-line"></div>
    </div>
    
    <!-- Floating Elements -->
    <div class="floating-elements">
      <div v-for="n in 30" :key="`element-${n}`" 
           class="floating-element"
           :style="getFloatingStyle()">
      </div>
    </div>

    <!-- Overlay -->
    <div class="background-overlay"></div>
  </div>
</template>

<script setup>
const getFloatingStyle = () => ({
  left: `${Math.random() * 100}%`,
  top: `${Math.random() * 100}%`,
  animationDelay: `${Math.random() * 5}s`,
  opacity: 0.1 + Math.random() * 0.2
})
</script>

<style scoped>
.modern-background {
  position: fixed;
  inset: 0;
  background: #000000;
  overflow: hidden;
  perspective: 1000px;
}

.orbs-container {
  position: absolute;
  inset: 0;
  filter: blur(60px);
}

.orb {
  position: absolute;
  border-radius: 50%;
  opacity: 0.5;
  animation: floatOrb 20s infinite ease-in-out;
}

.orb-1 {
  width: 50vw;
  height: 50vw;
  background: #675FF2;
  top: -25%;
  left: -10%;
  animation-delay: 0s;
}

.orb-2 {
  width: 40vw;
  height: 40vw;
  background: #DB3DCF;
  bottom: -20%;
  right: -10%;
  animation-delay: -5s;
}

.orb-3 {
  width: 45vw;
  height: 45vw;
  background: #3C397E;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -10s;
}

.grid-lines {
  position: absolute;
  inset: 0;
  opacity: 0.1;
  transform: perspective(1000px) rotateX(60deg);
}

.grid-line {
  position: absolute;
  width: 200%;
  height: 1px;
  background: linear-gradient(90deg, 
    transparent,
    #D0CCE3,
    transparent
  );
  animation: moveLines 10s linear infinite;
}

.floating-elements {
  position: absolute;
  inset: 0;
}

.floating-element {
  position: absolute;
  width: 4px;
  height: 4px;
  background: #D0CCE3;
  border-radius: 50%;
  animation: float 10s infinite ease-in-out;
}

.background-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(
    circle at center,
    transparent,
    rgba(0, 0, 0, 0.8)
  );
}

@keyframes floatOrb {
  0%, 100% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(-10%, -10%);
  }
}

@keyframes moveLines {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(100vh);
  }
}

@keyframes float {
  0%, 100% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(20px, -20px);
  }
}
</style>
```