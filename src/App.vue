<template>
  <div class="app">
    <div class="header">
      <h1>随机菜谱转盘</h1>
      <p>解决你不知道该吃什么的烦恼 qwq</p>
    </div>
    
    <div class="wheel-container">
      <div 
        class="wheel" 
        :class="{ spinning: isSpinning }"
        :style="{ transform: `rotate(${rotation}deg)` }"
      >
        <div class="wheel-center"></div>
        <div 
          v-for="(recipe, index) in recipes" 
          :key="index"
          class="wheel-segment"
          :style="{ transform: `rotate(${index * segmentAngle}deg)` }"
        >
          <span class="recipe-name">{{ recipe }}</span>
        </div>
        <div class="pointer"></div>
      </div>
    </div>
    
    <div class="controls">
      <button 
        @click="startSpin" 
        :disabled="isSpinning"
        class="spin-button"
      >
        {{ isSpinning ? '旋转中...' : '开始旋转' }}
      </button>
    </div>
    
    <div v-if="selectedRecipe" class="result">
      <div class="result-card">
        <h2>今日推荐</h2>
        <h3>屎</h3>
        <p>祝您用餐愉快！🍽️</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      recipes: ["屎", "水煮黑背鲈", "派蒙", "临渝炸鸡腿", "奶茶吨吨桶"],
      rotation: 0,
      isSpinning: false,
      selectedRecipe: null
    }
  },
  computed: {
    segmentAngle() {
      return 360 / this.recipes.length
    }
  },
  methods: {
    startSpin() {
      if (this.isSpinning) return
      
      this.isSpinning = true
      this.selectedRecipe = null
      
      const targetRotation = this.rotation + 1080 + Math.random() * 360
      const duration = 3000 + Math.random() * 2000
      
      this.animateSpin(targetRotation, duration)
    },
    
    animateSpin(targetRotation, duration) {
      const startTime = Date.now()
      const startRotation = this.rotation
      
      const animate = () => {
        const elapsed = Date.now() - startTime
        const progress = Math.min(elapsed / duration, 1)
        
        const easeOut = 1 - Math.pow(1 - progress, 3)
        
        this.rotation = startRotation + (targetRotation - startRotation) * easeOut
        
        if (progress < 1) {
          requestAnimationFrame(animate)
        } else {
          this.finishSpin()
        }
      }
      
      animate()
    },
    
    finishSpin() {
      this.isSpinning = false
      const normalizedRotation = (this.rotation % 360 + 360) % 360
      const segmentIndex = Math.floor(normalizedRotation / this.segmentAngle)
      this.selectedRecipe = this.recipes[segmentIndex]
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.app {
  min-height: 100vh;
  padding: 2rem;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.header {
  text-align: center;
  margin-bottom: 3rem;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 300;
  margin-bottom: 0.5rem;
  color: #ffffff;
}

.header p {
  font-size: 1.1rem;
  opacity: 0.8;
}

.wheel-container {
  position: relative;
  margin: 2rem 0;
}

.wheel {
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background: conic-gradient(
    from 0deg,
    #ff6b6b, #4ecdc4, #45b7d1, #96ceb4, 
    #feca57, #ff9ff3, #54a0ff, #ff6b6b
  );
  position: relative;
  transition: transform 0.1s linear;
  box-shadow: 0 0 50px rgba(255, 255, 255, 0.1);
}

.wheel.spinning {
  transition: transform 0.05s linear;
}

.wheel-center {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 40px;
  height: 40px;
  background: #1a1a2e;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  border: 3px solid #ffffff;
  z-index: 10;
}

.wheel-segment {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform-origin: center;
}

.recipe-name {
  position: absolute;
  top: 50px;
  left: 50%;
  transform: translateX(-50%) rotate(90deg);
  font-size: 0.9rem;
  font-weight: 600;
  color: #ffffff;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
  white-space: nowrap;
}

.pointer {
  position: absolute;
  top: -20px;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
  border-top: 30px solid #ff6b6b;
  z-index: 20;
}

.controls {
  margin: 2rem 0;
}

.spin-button {
  padding: 15px 40px;
  font-size: 1.2rem;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  color: white;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 5px 15px rgba(255, 107, 107, 0.4);
}

.spin-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(255, 107, 107, 0.6);
}

.spin-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.result {
  margin-top: 2rem;
  animation: fadeInUp 0.5s ease;
}

.result-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  text-align: center;
  max-width: 400px;
}

.result-card h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
  color: #4ecdc4;
}

.result-card h3 {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: #ff6b6b;
}

.result-card p {
  font-size: 1.1rem;
  opacity: 0.9;
  line-height: 1.6;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .wheel {
    width: 250px;
    height: 250px;
  }
  
  .header h1 {
    font-size: 2rem;
  }
  
  .spin-button {
    padding: 12px 30px;
    font-size: 1rem;
  }
}
</style>