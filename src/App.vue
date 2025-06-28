<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import ParallaxSection from "./components/ParallaxSection.vue";
import ScrollIndicator from "./components/ScrollIndicator.vue";

// 导入图片
import headLogo from "./assets/head-logo.png";
import title1 from "./assets/school-cover.jpg";
import title2 from "./assets/title-2.jpg";
import member1 from "./assets/members/chenjianheng.png";
import member2 from "./assets/members/wangtingting.jpg";
import member3 from "./assets/members/hujiahao.jpg";
import member4 from "./assets/members/chenjiayi.jpg";
import member5 from "./assets/members/dongzhile.jpg";
import member6 from "./assets/members/linjialing.jpg";

const isScrolled = ref(false);
const activeSection = ref("home");
const lightboxOpen = ref(false);
const lightboxImage = ref(1);
const mapContainer = ref(null);
let map = null;
const members = ref([
  {
    name: "陈健亨",
    image: member1,
    description: "指导老师",
  },
  {
    name: "王婷婷",
    image: member2,
    description: "指导老师",
  },
  {
    name: "胡家豪",
    image: member3,
    description: "参赛选手",
  },
  {
    name: "陈佳怡",
    image: member4,
    description: "参赛选手",
  },
  {
    name: "董志乐",
    image: member5,
    description: "参赛选手",
  },
  {
    name: "林佳玲",
    image: member6,
    description: "参赛选手",
  },
]);

// 获取图片URL的函数
const getImageUrl = (index) => {
  return new URL(`./assets/volunteer-pictures/${index}.jpg`, import.meta.url)
    .href;
};

// 初始化高德地图
const initMap = () => {
  if (typeof AMap !== "undefined" && mapContainer.value) {
    // 广州建设职业技术学院的坐标（从化区环市东路166号）
    const schoolLocation = [113.586, 23.548];

    map = new AMap.Map(mapContainer.value, {
      zoom: 15,
      center: schoolLocation,
      mapStyle: "amap://styles/normal",
    });

    // 添加标记点
    const marker = new AMap.Marker({
      position: schoolLocation,
      title: "广州建设职业技术学院",
    });

    // 添加信息窗体
    const infoWindow = new AMap.InfoWindow({
      content: `
        <div style="padding: 10px;">
          <h3 style="margin: 0 0 10px 0; color: #333;">广州建设职业技术学院</h3>
          <p style="margin: 0; color: #666;">广州市从化区环市东路166号</p>
        </div>
      `,
      offset: new AMap.Pixel(0, -30),
    });

    // 点击标记点显示信息窗体
    marker.on("click", () => {
      infoWindow.open(map, marker.getPosition());
    });

    map.add(marker);
  }
};

// 加载高德地图API
const loadAMap = () => {
  return new Promise((resolve, reject) => {
    if (typeof AMap !== "undefined") {
      resolve();
      return;
    }

    const script = document.createElement("script");
    script.src =
      "https://webapi.amap.com/maps?v=1.4.15&key=230cda1371b32eaf0928f97df9eec860";
    script.onload = resolve;
    script.onerror = reject;
    document.head.appendChild(script);
  });
};

// 平滑滚动效果
const scrollToSection = (sectionId) => {
  const targetSection = document.querySelector(`#${sectionId}`);
  if (targetSection) {
    targetSection.scrollIntoView({
      behavior: "smooth",
    });
  }
};

// 导航菜单切换
const toggleMobileMenu = () => {
  const navMenu = document.querySelector(".nav-menu");
  navMenu.classList.toggle("active");
};

// 监听滚动事件
const handleScroll = () => {
  isScrolled.value = window.pageYOffset > 100;

  // 更新活动section
  const sections = document.querySelectorAll(".section");
  sections.forEach((section) => {
    const rect = section.getBoundingClientRect();
    if (rect.top <= 100 && rect.bottom >= 100) {
      activeSection.value = section.id;
    }
  });
};

// 灯箱功能
const openLightbox = (imageIndex) => {
  lightboxImage.value = imageIndex;
  lightboxOpen.value = true;
  document.body.style.overflow = "hidden";
};

const closeLightbox = () => {
  lightboxOpen.value = false;
  document.body.style.overflow = "auto";
};

onMounted(async () => {
  window.addEventListener("scroll", handleScroll);

  // 导航链接点击事件
  const navLinks = document.querySelectorAll(".nav-menu a");
  navLinks.forEach((link) => {
    link.addEventListener("click", (e) => {
      e.preventDefault();
      const targetId = link.getAttribute("href").substring(1);
      scrollToSection(targetId);
    });
  });

  // 初始化地图
  try {
    await loadAMap();
    // 延迟初始化地图，确保DOM已经渲染
    setTimeout(() => {
      initMap();
    }, 100);
  } catch (error) {
    console.error("Failed to load AMap:", error);
  }
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<template>
  <div id="app">
    <!-- 滚动指示器 -->
    <ScrollIndicator />

    <!-- 导航栏 -->
    <nav class="navbar" :class="{ 'navbar-scrolled': isScrolled }">
      <div class="nav-container">
        <div class="nav-logo">
          <img :src="headLogo" alt="Logo" />
        </div>
        <ul class="nav-menu">
          <li>
            <a href="#home" :class="{ active: activeSection === 'home' }"
              >首页</a
            >
          </li>
          <li>
            <a href="#about" :class="{ active: activeSection === 'about' }"
              >关于我们</a
            >
          </li>
          <li>
            <a href="#gallery" :class="{ active: activeSection === 'gallery' }"
              >图片展示</a
            >
          </li>
          <li>
            <a href="#contact" :class="{ active: activeSection === 'contact' }"
              >联系我们</a
            >
          </li>
        </ul>
        <div class="nav-toggle" @click="toggleMobileMenu">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
    </nav>

    <!-- 主要内容区域 -->
    <main class="main-content">
      <!-- 首页section -->
      <section id="home" class="section hero-section">
        <div class="hero-content"></div>
        <div class="hero-background">
          <img :src="title1" alt="Hero Background" />
        </div>
        <div class="scroll-hint">
          <div class="scroll-arrow"></div>
          <span>向下滚动</span>
        </div>
      </section>

      <!-- 关于我们section -->
      <section id="about" class="section about-section">
        <div class="section-content">
          <div class="text-content">
            <h2 class="section-title">关于我们</h2>
            <div class="members-grid">
              <div
                class="member-item"
                v-for="member in members"
                :key="member.name"
              >
                <div class="member-image">
                  <img :src="member.image" :alt="member.name" />
                </div>
                <div class="member-info">
                  <h3>{{ member.name }}</h3>
                  <p>{{ member.description }}</p>
                </div>
              </div>
            </div>
          </div>
          <div class="image-content">
            <img :src="title2" alt="About Us" />
          </div>
        </div>
      </section>

      <!-- 图片展示section -->
      <section id="gallery" class="section gallery-section">
        <div class="gallery-container">
          <div class="gallery-header">
            <h2 class="section-title">精彩瞬间</h2>
            <p>记录每一个美好时刻，展现学校的精彩生活</p>
            <p>
              这里有学生们的学习时光、活动瞬间、成长足迹，每一张照片都承载着美好的回忆。
            </p>
          </div>
          <div class="gallery-grid">
            <div
              class="gallery-item"
              v-for="i in 16"
              :key="i"
              @click="openLightbox(i)"
            >
              <img :src="getImageUrl(i)" :alt="`Gallery Image ${i}`" />
              <div class="gallery-overlay">
                <h3>精彩瞬间 {{ i }}</h3>
                <p>美好的回忆</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- 联系我们section -->
      <section id="contact" class="section contact-section">
        <div class="contact-content">
          <h2 class="section-title">联系我们</h2>
          <div class="map" ref="mapContainer"></div>
        </div>
      </section>
    </main>

    <!-- 图片灯箱 -->
    <div v-if="lightboxOpen" class="lightbox" @click="closeLightbox">
      <div class="lightbox-content" @click.stop>
        <img :src="getImageUrl(lightboxImage)" alt="Lightbox Image" />
        <button class="lightbox-close" @click="closeLightbox">&times;</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 导航栏增强样式 */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  z-index: 1000;
  transition: all 0.3s ease;
  border-bottom: 1px solid transparent;
}

.navbar-scrolled {
  background: rgba(255, 255, 255, 0.98);
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.1);
}

.nav-container {
  max-width: 1200px;
  margin: 0 30px;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 60px;
}

.nav-logo img {
  height: 40px;
  width: auto;
}

.nav-menu {
  display: flex;
  list-style: none;
  gap: 30px;
}

.nav-menu a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
  transition: all 0.3s ease;
  position: relative;
}

.nav-menu a:hover,
.nav-menu a.active {
  color: #007aff;
}

.nav-menu a::after {
  content: "";
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: #007aff;
  transition: width 0.3s ease;
}

.nav-menu a:hover::after,
.nav-menu a.active::after {
  width: 100%;
}

.nav-toggle {
  display: none;
  flex-direction: column;
  cursor: pointer;
  gap: 4px;
}

.nav-toggle span {
  width: 25px;
  height: 3px;
  background: #333;
  transition: all 0.3s ease;
}

/* 主要内容区域 */
.main-content {
  margin-top: 60px;
}

/* Section 通用样式 */
.section {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: 80px 0;
  box-sizing: border-box;
}

.section-title {
  font-size: 3rem;
  margin-bottom: 30px;
  color: #333;
  position: relative;
}

.section-title::after {
  content: "";
  position: absolute;
  bottom: -10px;
  left: 0;
  width: 60px;
  height: 3px;
  background: #007aff;
}

/* 首页section增强 */
.hero-section {
  color: white;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.hero-content {
  z-index: 2;
  position: relative;
}

.hero-title {
  font-size: 4rem;
  font-weight: 700;
  margin-bottom: 20px;
  animation: fadeInUp 1s ease;
}

.hero-subtitle {
  font-size: 1.5rem;
  margin-bottom: 40px;
  opacity: 0.9;
  animation: fadeInUp 1s ease 0.2s both;
}

.cta-button {
  background: #007aff;
  color: white;
  border: none;
  padding: 15px 40px;
  font-size: 1.2rem;
  border-radius: 30px;
  cursor: pointer;
  transition: all 0.3s ease;
  animation: fadeInUp 1s ease 0.4s both;
}

.cta-button:hover {
  background: #0056cc;
  transform: translateY(-2px);
  box-shadow: 0 10px 30px rgba(0, 122, 255, 0.3);
}

.hero-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.hero-background img {
  width: 100%;
  height: 100%;
}

.scroll-hint {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  color: white;
  text-align: center;
  animation: bounce 2s infinite;
}

.scroll-arrow {
  width: 20px;
  height: 20px;
  border-right: 2px solid white;
  border-bottom: 2px solid white;
  transform: rotate(45deg);
  margin: 0 auto 10px;
}

/* 关于我们section增强 */
.about-section {
  background: #f8f9fa;
  position: relative;
}

.section-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: center;
  width: 100%;
}

.members-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  margin-top: 30px;
}

.member-item {
  display: flex;
  align-items: center;
  padding: 15px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.member-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.member-image {
  width: 80px;
  height: 100px;
  border-radius: 8px;
  overflow: hidden;
  margin-right: 15px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
}

.member-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.member-info h3 {
  font-size: 1.1rem;
  color: #333;
  margin-bottom: 5px;
  font-weight: 600;
}

.member-info p {
  font-size: 0.9rem;
  color: #666;
  margin: 0;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 30px;
  margin-top: 40px;
}

.stat-item {
  text-align: center;
  padding: 20px;
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.stat-item:hover {
  transform: translateY(-5px);
}

.stat-item h3 {
  font-size: 2.5rem;
  color: #007aff;
  margin-bottom: 10px;
}

.stat-item p {
  color: #666;
  font-size: 1rem;
}

.image-content img {
  width: 100%;
  object-fit: contain;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.image-content img:hover {
  transform: scale(1.05);
}

/* 图片展示section增强 */
.gallery-section {
  background: white;
  position: relative;
}

.gallery-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  width: 100%;
}

.gallery-header {
  text-align: left;
  margin-bottom: 60px;
}

.gallery-header p {
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 15px;
  line-height: 1.6;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  width: 100%;
}

.gallery-item {
  position: relative;
  overflow: hidden;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  cursor: pointer;
  aspect-ratio: 1;
}

.gallery-item:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2);
}

.gallery-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.gallery-item:hover img {
  transform: scale(1.1);
}

.gallery-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(transparent, rgba(0, 0, 0, 0.8));
  color: white;
  padding: 30px 20px 20px;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}

.gallery-item:hover .gallery-overlay {
  transform: translateY(0);
}

.gallery-overlay h3 {
  font-size: 1.2rem;
  margin-bottom: 10px;
}

.gallery-overlay p {
  font-size: 0.9rem;
  opacity: 0.8;
}

/* 联系我们section增强 */
.contact-section {
  background: #f8f9fa;
  color: #333;
  position: relative;
}

.contact-content {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.contact-content h2 {
  font-size: 3rem;
  margin-bottom: 60px;
  color: #333;
}

.map {
  width: 100%;
  height: 500px;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.contact-info {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 60px;
  align-items: start;
}

.contact-details {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.contact-item {
  display: flex;
  align-items: center;
  padding: 20px;
  background: white;
  border-radius: 15px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.contact-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
}

.contact-icon {
  font-size: 2rem;
  margin-right: 20px;
  width: 50px;
  text-align: center;
}

.contact-item h3 {
  font-size: 1.3rem;
  margin-bottom: 8px;
  color: #333;
  font-weight: 600;
}

.contact-item p {
  font-size: 1.1rem;
  color: #666;
  margin: 0;
  line-height: 1.5;
}

/* 灯箱样式 */
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.9);
  z-index: 2000;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.3s ease;
}

.lightbox-content {
  position: relative;
  max-width: 90%;
  max-height: 90%;
}

.lightbox-content img {
  width: 100%;
  height: auto;
  object-fit: cover;
  border-radius: 10px;
}

.lightbox-close {
  position: absolute;
  top: -40px;
  right: 0;
  background: none;
  border: none;
  color: white;
  font-size: 2rem;
  cursor: pointer;
  padding: 10px;
}

/* 动画 */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {
  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateX(-50%) translateY(0);
  }
  40% {
    transform: translateX(-50%) translateY(-10px);
  }
  60% {
    transform: translateX(-50%) translateY(-5px);
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .nav-menu {
    position: fixed;
    top: 60px;
    left: -100%;
    width: 100%;
    height: calc(100vh - 60px);
    background: rgba(255, 255, 255, 0.98);
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    padding-top: 50px;
    transition: left 0.3s ease;
  }

  .nav-menu.active {
    left: 0;
  }

  .nav-toggle {
    display: flex;
  }

  .hero-title {
    font-size: 2.5rem;
  }

  .section {
    padding: 60px 0;
  }

  .section-content {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .stats {
    grid-template-columns: 1fr;
  }

  .members-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .member-item {
    padding: 12px;
  }

  .member-image {
    width: 60px;
    height: 80px;
    margin-right: 12px;
  }

  .member-info h3 {
    font-size: 1rem;
  }

  .member-info p {
    font-size: 0.8rem;
  }

  .gallery-header {
    margin-bottom: 40px;
  }

  .gallery-header p {
    font-size: 1rem;
  }

  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
  }

  .contact-info {
    grid-template-columns: 1fr;
  }

  .map {
    height: 300px;
    margin-bottom: 30px;
  }

  .contact-details {
    gap: 20px;
  }

  .contact-item {
    padding: 15px;
  }

  .contact-icon {
    font-size: 1.5rem;
    margin-right: 15px;
    width: 40px;
  }

  .contact-item h3 {
    font-size: 1.1rem;
  }

  .contact-item p {
    font-size: 1rem;
  }
}

@media (max-width: 480px) {
  .gallery-section {
    padding: 40px 0;
  }

  .gallery-header {
    margin-bottom: 30px;
  }

  .gallery-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .gallery-overlay {
    padding: 20px 15px 15px;
  }

  .gallery-overlay h3 {
    font-size: 1rem;
  }

  .gallery-overlay p {
    font-size: 0.8rem;
  }
}

/* 滚动条样式 */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #007aff;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #0056cc;
}
</style>
