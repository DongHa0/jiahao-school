<template>
  <div class="parallax-section" :style="parallaxStyle">
    <slot></slot>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const props = defineProps({
  speed: {
    type: Number,
    default: 0.5,
  },
});

const parallaxStyle = ref({});
const scrolled = ref(0);

const handleScroll = () => {
  scrolled.value = window.pageYOffset;
  const yPos = -(scrolled.value * props.speed);
  parallaxStyle.value = {
    transform: `translateY(${yPos}px)`,
  };
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<style scoped>
.parallax-section {
  position: relative;
  will-change: transform;
  width: 100%;
  height: 100%;
}
</style>
