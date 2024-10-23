<template>
    <div 
      class="movie-card"
      :class="{
        'dragging': isDragging && index === 0,
        'exit-left': exitDirection === 'left' && index === 0,
        'exit-right': exitDirection === 'right' && index === 0
      }"
      :style="cardStyle"
      @mousedown="index === 0 ? startDrag($event) : undefined"
      @mousemove="index === 0 ? onDrag($event) : undefined"
      @mouseup="index === 0 ? endDrag() : undefined"
      @mouseleave="index === 0 ? endDrag() : undefined"
      @touchstart="index === 0 ? startTouchDrag($event) : undefined"
      @touchmove.prevent="index === 0 ? onTouchDrag($event) : undefined"
      @touchend="index === 0 ? endDrag() : undefined"
    >
      <!-- Holographic Overlay -->
      <div class="holographic-overlay"></div>
      
      <!-- Movie Content -->
      <div class="card-content">
        <img 
          :src="movie.imageUrl" 
          :alt="movie.title"
          class="movie-poster"
          draggable="false"
        />
        <div class="movie-info">
          <h2 class="movie-title">{{ movie.title }}</h2>
          <p class="movie-year">{{ movie.year }}</p>
          <div class="movie-rating">
            <span class="rating-stars">★★★★★</span>
            <span class="rating-number">{{ movie.rating }}</span>
          </div>
          <!-- <p class="movie-description">{{ movie.description }}</p> -->
        </div>
      </div>
  
      <!-- Swipe Indicators -->
      <div 
        class="swipe-indicator like"
        :style="{ opacity: getLikeOpacity }"
      >
        LIKE
      </div>
      <div 
        class="swipe-indicator nope"
        :style="{ opacity: getNopeOpacity }"
      >
        NOPE
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, defineProps, defineEmits } from 'vue'
  
  const props = defineProps({
    movie: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    exitDirection: {
      type: String,
      default: null
    }
  })
  
  const emit = defineEmits(['swipe'])
  
  // Drag state
  const isDragging = ref(false)
  const startX = ref(0)
  const currentX = ref(0)
  
  // Computed styles
  const cardStyle = computed(() => {
    const isCurrentCard = props.index === 0
    const isNextCard = props.index === 1
    const isThirdCard = props.index === 2
  
    // Handle exit animations
    if (props.exitDirection && isCurrentCard) {
      const translateX = props.exitDirection === 'right' ? 1000 : -1000
      return {
        transform: `translateX(${translateX}px) rotate(${translateX * 0.1}deg)`,
        transition: 'transform 0.3s ease',
        opacity: 0,
        zIndex: 3
      }
    }
  
    // Handle dragging
    if (isDragging.value && isCurrentCard) {
      const rotate = currentX.value * 0.1
      return {
        transform: `translateX(${currentX.value}px) rotate(${rotate}deg)`,
        transition: 'none',
        zIndex: 3
      }
    }
  
    // Handle stacking
    if (isCurrentCard) {
      return {
        transform: 'translate(0, 0) rotate(0deg)',
        opacity: 1,
        zIndex: 3
      }
    } else if (isNextCard) {
      return {
        transform: 'translate(0, 4px) scale(0.95)',
        opacity: 0.8,
        zIndex: 2
      }
    } else if (isThirdCard) {
      return {
        transform: 'translate(0, 8px) scale(0.9)',
        opacity: 0.6,
        zIndex: 1
      }
    }
  
    return {
      transform: 'translate(0, 12px) scale(0.85)',
      opacity: 0.4,
      zIndex: 0
    }
  })
  
  const getLikeOpacity = computed(() => Math.max(0, currentX.value / 100))
  const getNopeOpacity = computed(() => Math.max(0, -currentX.value / 100))
  
  // Drag handlers
  const startDrag = (event) => {
    isDragging.value = true
    startX.value = event.clientX - currentX.value
  }
  
  const onDrag = (event) => {
    if (!isDragging.value) return
    currentX.value = event.clientX - startX.value
  }
  
  const startTouchDrag = (event) => {
    isDragging.value = true
    startX.value = event.touches[0].clientX - currentX.value
  }
  
  const onTouchDrag = (event) => {
    if (!isDragging.value) return
    currentX.value = event.touches[0].clientX - startX.value
  }
  
  const endDrag = () => {
    if (!isDragging.value) return
    isDragging.value = false
    
    const threshold = 100
    if (Math.abs(currentX.value) > threshold) {
      emit('swipe', currentX.value > 0 ? 'right' : 'left')
    }
    currentX.value = 0
  }
  </script>
  
  <style scoped>
  .movie-card {
    position: absolute;
    width: 320px;
    height: 480px;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 16px;
    overflow: hidden;
    backdrop-filter: blur(10px);
    cursor: grab;
    user-select: none;
    transform-origin: 50% 100%;
    will-change: transform;
    transition: all 0.3s ease;
    margin-top: 150px;
  }
  
  .movie-card.dragging {
    cursor: grabbing;
    transition: none;
  }
  
  .holographic-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      135deg,
      transparent,
      rgba(77, 181, 255, 0.1),
      transparent
    );
    animation: holographic 3s linear infinite;
  }
  
  .card-content {
    height: 100%;
    position: relative;
    z-index: 1;
  }
  
  .movie-poster {
    width: 100%;
    height: 70%;
    object-fit: cover;
  }
  
  .movie-info {
    padding: 1rem;
    background: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(10px);
  }
  
  .movie-title {
    font-size: 1.5rem;
    font-weight: bold;
    margin-bottom: 0.5rem;
    background: linear-gradient(45deg, #4facfe, #00f2fe);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  
  .movie-rating {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    margin: 0.5rem 0;
  }
  
  .rating-stars {
    color: #ffd700;
  }
  
  .swipe-indicator {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    padding: 1rem 2rem;
    border-radius: 999px;
    font-weight: bold;
    font-size: 1.5rem;
    pointer-events: none;
    z-index: 2;
  }
  
  .like {
    right: 2rem;
    background: rgba(76, 175, 80, 0.9);
    color: white;
    transform: rotate(15deg);
  }
  
  .nope {
    left: 2rem;
    background: rgba(255, 82, 82, 0.9);
    color: white;
    transform: rotate(-15deg);
  }
  
  @keyframes holographic {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
  }
  
  .exit-left {
    animation: exitLeft 0.3s ease forwards;
  }
  
  .exit-right {
    animation: exitRight 0.3s ease forwards;
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