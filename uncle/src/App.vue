<template>
  <div class="app-container">
    <div class="decorative-text left">山衔</div>
    <div class="decorative-text right">雁引</div>
    <div class="content-wrapper">
      <h1 class="title">和侄子今天做什么？</h1>
      <ImageSelector />
    </div>
  </div>
</template>

<script>
import ImageSelector from './components/ImageSelector.vue'
import backgroundImage from '@/assets/nanke.jpg'

export default {
  name: 'App',
  components: {
    ImageSelector
  },
  data() {
    return {
      backgroundStyle: {
        background: `url(${backgroundImage}) no-repeat center center fixed`,
        backgroundSize: 'cover'
      }
    }
  }
}
</script>

<style>
.app-container {
  min-height: 100vh;
  padding: 20px;
  background: url('../assets/nanke.jpg') no-repeat center center fixed;
  background-size: cover;
  padding: 20px;
}

.content-wrapper {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.decorative-text {
  position: absolute;
  font-size: 72px;
  font-family: '楷体', KaiTi, serif;
  color: rgba(255, 255, 255, 0.85);
  writing-mode: vertical-rl;
  text-orientation: upright;
  letter-spacing: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5),
    0 0 20px rgba(255, 255, 255, 0.3),
    0 0 40px rgba(255, 255, 255, 0.2);
  pointer-events: none;
}

.decorative-text.left {
  left: 60px;
  top: 50%;
  transform: translateY(-50%);
}

.decorative-text.right {
  right: 60px;
  top: 50%;
  transform: translateY(-50%);
}

.title {
  color: #2c3e50;
  text-align: center;
  margin-bottom: 40px;
  font-size: 2.5rem;
  font-weight: 600;
}

@media (max-width: 768px) {
  .title {
    font-size: 1.8rem;
    margin-bottom: 20px;
  }

  .decorative-text {
    font-size: 48px;
  }

  .decorative-text.left {
    left: 30px;
  }

  .decorative-text.right {
    right: 30px;
  }
}
</style>
