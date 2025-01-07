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

export default {
  name: 'App',
  components: {
    ImageSelector
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100vh;
  overflow: hidden;
}

.app-container {
  min-height: 100vh;
  padding: 20px;
  position: relative;
}

.app-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('@/assets/nanke.jpg') no-repeat center center fixed;
  background-size: cover;
  filter: blur(3px) brightness(0.9);
  z-index: -1;
}

.content-wrapper {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  position: relative;
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
  font-size: 2.5rem;
  color: #fff;
  text-align: center;
  margin-bottom: 2rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  background: rgba(0, 0, 0, 0.4);
  padding: 1rem;
  border-radius: 10px;
  backdrop-filter: blur(5px);
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
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
