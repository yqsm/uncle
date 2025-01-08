<template>
  <div class="app-container">
    <FloatingText />
    <div class="content-wrapper">
      <h1 class="title">和侄子今天做什么？</h1>
      <ImageSelector />
    </div>
  </div>
</template>

<script>
import ImageSelector from './components/ImageSelector.vue'
import FloatingText from './components/FloatingText.vue'

export default {
  name: 'App',
  components: {
    ImageSelector,
    FloatingText
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
  z-index: 1;
}

.title {
  font-size: 2.5rem;
  color: #fff;
  text-align: center;
  margin-bottom: 30px;
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
