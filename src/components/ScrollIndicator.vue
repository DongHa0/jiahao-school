<template>
  <div class="scroll-indicator">
    <div class="scroll-progress" :style="{ width: scrollProgress + '%' }"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const scrollProgress = ref(0);

const calculateScrollProgress = () => {
  const scrollTop = window.pageYOffset;
  const docHeight = document.documentElement.scrollHeight - window.innerHeight;
  scrollProgress.value = (scrollTop / docHeight) * 100;
};

onMounted(() => {
  window.addEventListener("scroll", calculateScrollProgress);
  calculateScrollProgress();
});

onUnmounted(() => {
  window.removeEventListener("scroll", calculateScrollProgress);
});
</script>

<style scoped>
.scroll-indicator {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 3px;
  background: rgba(255, 255, 255, 0.2);
  z-index: 1001;
}

.scroll-progress {
  height: 100%;
  background: linear-gradient(90deg, #007aff, #00c6ff);
  transition: width 0.1s ease;
}
</style>
