<template>
  <div class="card">
    <div class="image-display" ref="imageDisplayRef">
      <div
        v-for="(image, index) in images"
        :key="index"
        class="image-wrapper"
        :style="{
          opacity: currentIndex === index ? 1 : 0,
          transform: `scale(${currentIndex === index ? 1 : 0.95})`,
          zIndex: currentIndex === index ? 1 : 0
        }"
      >
        <img
          :src="image.url"
          :alt="image.name"
          @load="handleImageLoad(index)"
          :class="{ loaded: loadedImages[index] }"
        />
      </div>
    </div>

    <div class="controls">
      <button
        @click="startSelection"
        :disabled="isSelecting"
        class="select-button"
        :class="{ disabled: isSelecting }"
      >
        {{ buttonText }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref, reactive, computed } from 'vue'

const imageContext = import.meta.glob('@/assets/images/*.{JPG,jpg,PNG,png}', { eager: true })

export default {
  name: 'ImageSelector',

  setup() {
    const images = Object.entries(imageContext).map(([path, module]) => {
      const name = path.split('/').pop().split('.')[0]
      return {
        id: name,
        url: module.default,
        name: `活动${name.slice(-4)}`
      }
    })

    const currentIndex = ref(0)
    const isSelecting = ref(false)
    const loadedImages = reactive({})

    const buttonText = computed(() => {
      return isSelecting.value ? '选择中...' : '开始选择'
    })

    const handleImageLoad = (index) => {
      loadedImages[index] = true
    }

    const sleep = (ms) => {
      return new Promise(resolve => setTimeout(resolve, ms))
    }

    const easeOutQuad = (t) => {
      return t * (2 - t)
    }

    const startSelection = async () => {
      if (isSelecting.value) return

      isSelecting.value = true
      const totalDuration = 3000
      const initialSpeed = 50
      const finalSpeed = 300
      let currentSpeed = initialSpeed
      
      const startTime = Date.now()

      while (Date.now() - startTime < totalDuration) {
        const progress = (Date.now() - startTime) / totalDuration
        currentSpeed = initialSpeed + (finalSpeed - initialSpeed) * easeOutQuad(progress)
        
        let newIndex
        do {
          newIndex = Math.floor(Math.random() * images.length)
        } while (newIndex === currentIndex.value)
        
        currentIndex.value = newIndex
        await sleep(currentSpeed)
      }

      const finalIndex = Math.floor(Math.random() * images.length)
      currentIndex.value = finalIndex
      isSelecting.value = false
    }

    // 预加载所有图片
    images.forEach((image, index) => {
      const img = new Image()
      img.src = image.url
      img.onload = () => handleImageLoad(index)
    })

    return {
      images,
      currentIndex,
      isSelecting,
      loadedImages,
      buttonText,
      handleImageLoad,
      startSelection
    }
  }
}
</script>

<style scoped>
.card {
  background: rgba(255, 255, 255, 0.95);
  border-radius: 8px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  padding: 24px;
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  position: relative;
}

.card::before {
  content: '';
  position: absolute;
  top: 4px;
  right: 4px;
  bottom: 4px;
  left: 4px;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 6px;
  pointer-events: none;
}

.image-display {
  aspect-ratio: 1;
  position: relative;
  overflow: hidden;
  border-radius: 4px;
  background: #f5f5f5;
  margin-bottom: 24px;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.image-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.image-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.controls {
  text-align: center;
  padding: 8px 0;
  position: relative;
}

.select-button {
  background: transparent;
  color: #4a4a4a;
  border: 1px solid #4a4a4a;
  padding: 12px 36px;
  border-radius: 4px;
  font-size: 1.1rem;
  font-family: inherit;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  letter-spacing: 2px;
}

.select-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: rgba(74, 74, 74, 0.1);
  transition: all 0.3s ease;
}

.select-button:hover:not(.disabled) {
  color: #2c3e50;
  border-color: #2c3e50;
}

.select-button:hover:not(.disabled)::before {
  left: 0;
}

.select-button.disabled {
  background: transparent;
  border-color: #ccc;
  color: #ccc;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .card {
    margin: 10px;
    padding: 16px;
  }

  .select-button {
    padding: 10px 28px;
    font-size: 1rem;
  }
}
</style>
