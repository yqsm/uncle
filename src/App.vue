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
  color: rgba(255, 217, 102, 0.9);
  -webkit-writing-mode: vertical-rl;
  writing-mode: vertical-rl;
  z-index: 1;
  padding: 0.5em 0.3em;
  text-shadow: 
    2px 2px 0 rgba(255, 166, 0, 0.4),
    -2px -2px 0 rgba(255, 255, 255, 0.4);
  line-height: 1.2;
  max-height: 4em;
  overflow: hidden;
  text-align: center;
  top: 50%;
  -webkit-transform: translateY(-50%);
  transform: translateY(-50%);
  font-weight: 600;
  letter-spacing: 0.1em;
}

.left {
  left: -0.2em;
}

.right {
  right: -0.2em;
}

@media screen and (max-width: 768px) {
  .decorative-text {
    position: absolute;
    font-size: 48px;
    color: rgba(255, 230, 140, 1);
    text-shadow: 
      2px 2px 4px rgba(0, 0, 0, 0.3),
      -1px -1px 0 rgba(255, 255, 255, 0.5);
    padding: 0;
  }
  
  .left {
    left: 0;
    margin-left: 10px;
    text-align: start;
  }
  
  .right {
    right: 0;
    margin-right: 10px;
    text-align: end;
  }
}

@media screen and (max-width: 480px) {
  .decorative-text {
    font-size: 36px;
  }
  
  .left {
    margin-left: 5px;
  }
  
  .right {
    margin-right: 5px;
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
