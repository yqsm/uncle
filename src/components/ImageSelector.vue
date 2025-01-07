<template>
  <div class="card">
    <div class="image-display" ref="imageDisplayRef">
      <div
        v-for="(image, index) in images"
        :key="index"
        v-show="currentIndex === index"
        class="image-wrapper"
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
import img1 from '@/assets/images/IMG_8081.JPG'
import img2 from '@/assets/images/IMG_8082.JPG'
import img3 from '@/assets/images/IMG_8083.JPG'
import img4 from '@/assets/images/IMG_8084.JPG'

export default {
  name: 'ImageSelector',

  data() {
    return {
      images: [
        {
          id: 1,
          url: img1,
          name: '活动1'
        },
        {
          id: 2,
          url: img2,
          name: '活动2'
        },
        {
          id: 3,
          url: img3,
          name: '活动3'
        },
        {
          id: 4,
          url: img4,
          name: '活动4'
        }
      ],
      currentIndex: 0,
      isSelecting: false,
      loadedImages: {}
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

    startSelection() {
      if (this.isSelecting) return

      this.isSelecting = true
      const duration = 2000
      let startTime = null

      const animate = timestamp => {
        if (!startTime) startTime = timestamp
        const progress = timestamp - startTime

        if (progress < duration) {
          this.currentIndex = Math.floor(
            Math.random() * this.images.length
          )
          requestAnimationFrame(animate)
        } else {
          const finalIndex = Math.floor(
            Math.random() * this.images.length
          )
          this.currentIndex = finalIndex
          this.isSelecting = false
        }
      }

      requestAnimationFrame(animate)
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
  opacity: 0;
  transition: opacity 0.3s ease;
}

.image-wrapper:not([v-show='false']) {
  opacity: 1;
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
