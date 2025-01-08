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
  display: flex;
  align-items: center;
  justify-content: center;
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
  width: 100%;
  padding: 20px;
  position: relative;
}

.decorative-text {
  position: fixed;
  font-size: 72px;
  color: rgba(255, 255, 255, 0.8);
  writing-mode: vertical-rl;
  z-index: 1;
  padding: 0.5em 0.3em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  line-height: 1.2;
  max-height: 4em;
  overflow: hidden;
  text-align: center;
  top: 50%;
  transform: translateY(-50%);
}

.left {
  left: -0.2em;
}

.right {
  right: -0.2em;
}

@media screen and (max-width: 768px) {
  .decorative-text {
    font-size: 48px;
    background-color: rgba(0, 0, 0, 0.3);
    border-radius: 0.2em;
  }
  
  .left {
    left: -0.25em;
  }
  
  .right {
    right: -0.25em;
  }
}

@media screen and (max-width: 480px) {
  .decorative-text {
    font-size: 36px;
    padding: 0.4em 0.2em;
    max-height: 3em;
  }
  
  .left {
    left: -0.3em;
  }
  
  .right {
    right: -0.3em;
  }
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

@media screen and (max-width: 768px) {
  .title {
    font-size: 1.8rem;
    margin-bottom: 20px;
  }
}
</style>
