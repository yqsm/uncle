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
const imageContext = import.meta.glob('@/assets/images/*.{JPG,jpg,PNG,png}', { eager: true })

export default {
  name: 'ImageSelector',

  data() {
    const images = Object.entries(imageContext).map(([path, module]) => {
      const name = path.split('/').pop().split('.')[0]
      return {
        id: name,
        url: module.default,
        name: `活动${name.slice(-4)}`  // 使用文件名最后4位作为活动编号
      }
    })

    return {
      images,
      currentIndex: 0,
      isSelecting: false,
      loadedImages: {},
      animationSpeed: 100  // 控制动画速度（毫秒）
    }
  },

  computed: {
    buttonText() {
      return this.isSelecting ? '选择中...' : '开始选择'
    }
  },

  methods: {
    handleImageLoad(index) {
      this.$set(this.loadedImages, index, true)
    },

    async startSelection() {
      if (this.isSelecting) return

      this.isSelecting = true
      const totalDuration = 3000  // 总动画时长
      const initialSpeed = 50     // 初始切换间隔（毫秒）
      const finalSpeed = 300      // 最终切换间隔（毫秒）
      let currentSpeed = initialSpeed
      
      const startTime = Date.now()

      while (Date.now() - startTime < totalDuration) {
        // 计算动画进度（0到1之间）
        const progress = (Date.now() - startTime) / totalDuration
        
        // 使用easeOut函数使动画逐渐减速
        currentSpeed = initialSpeed + (finalSpeed - initialSpeed) * this.easeOutQuad(progress)
        
        // 随机选择一个不同的索引
        let newIndex
        do {
          newIndex = Math.floor(Math.random() * this.images.length)
        } while (newIndex === this.currentIndex)
        
        this.currentIndex = newIndex
        
        // 等待当前速度对应的时间
        await this.sleep(currentSpeed)
      }

      // 最后一次随机
      const finalIndex = Math.floor(Math.random() * this.images.length)
      this.currentIndex = finalIndex
      this.isSelecting = false
    },

    sleep(ms) {
      return new Promise(resolve => setTimeout(resolve, ms))
    },

    easeOutQuad(t) {
      return t * (2 - t)  // 二次方缓动函数
    }
  },

  mounted() {
    // 预加载所有图片
    this.images.forEach((image, index) => {
      const img = new Image()
      img.src = image.url
      img.onload = () => this.handleImageLoad(index)
    })
  }
}
</script>

<style scoped>
.card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
}

.image-display {
  aspect-ratio: 1;
  position: relative;
  overflow: hidden;
  border-radius: 8px;
  background: #f5f5f5;
  margin-bottom: 20px;
}

.image-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: all 0.3s ease;
}

.image-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.controls {
  text-align: center;
  padding: 20px 0;
}

.select-button {
  background: #42b883;
  color: white;
  border: none;
  padding: 12px 30px;
  border-radius: 25px;
  font-size: 1.1rem;
  font-family: inherit;  /* 继承父元素的字体 */
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(66, 184, 131, 0.2);
}

.select-button:hover:not(.disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 8px rgba(66, 184, 131, 0.3);
}

.select-button.disabled {
  background: #ccc;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

@media (max-width: 768px) {
  .card {
    margin: 10px;
  }

  .select-button {
    padding: 10px 24px;
    font-size: 1rem;
  }
}
</style>
