<template>
  <div class="back" :style="{ minHeight: backHeight + 'px' }">
    <!-- 背景轮播层：随滚动交叉淡入淡出 -->
    <div
      v-for="(bg, idx) in backgrounds"
      :key="idx"
      class="bg-layer"
      :style="{ backgroundImage: `url(${bg})`, opacity: layerOpacity(idx) }"
    ></div>

    <div class="viewport">
      <div class="blog-card">
        <!-- 头像 -->
        <img v-if="avatar" :src="avatar" alt="avatar" class="avatar" />

        <!-- 基本信息 -->
        <h1>{{ name }}</h1>
        <p class="bio">{{ bio }}</p>

        <!-- 社交链接 -->
        <div class="social-links" v-if="socialLinks.length">
          <a
            v-for="link in socialLinks"
            :key="link.name"
            :href="link.url"
            target="_blank"
            rel="noopener noreferrer"
          >
            {{ link.name }}
          </a>
        </div>

        <!-- 文章列表 -->
        <div class="posts" v-if="posts.length">
          <h2>📝 最新文章</h2>
          <ul>
            <li v-for="post in posts" :key="post.id">
              <a :href="post.link">{{ post.title }}</a>
              <span class="date">{{ post.date }}</span>
            </li>
          </ul>
        </div>

        <!-- 技能标签 -->
        <div class="skills" v-if="skills.length">
          <h2>🛠️ 技术栈</h2>
          <span v-for="skill in skills" :key="skill" class="skill-tag">
            {{ skill }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import avatar from '../assets/picture/avatar.jpeg'

// ===== 背景图片数组：在这里接着往下加即可 =====
// 例如：import bg3 from '../assets/picture/bg3.jpg' 然后 push 进 backgrounds
import bg1 from '../assets/picture/1.jpg'
import bg2 from '../assets/picture/2.jpg'
import bg3 from '../assets/picture/3.jpg'

const backgrounds = [bg1, bg2, bg3]
const N = backgrounds.length

// ===== 可调参数 =====
// 每张图占多少屏的滚动距离：越大，这张图停留/滚得越久
const SCROLL_PER_IMAGE = 3
// 每张图段的“尾部”多久开始渐变（0~1）：越大渐变区间越长；越小越接近“瞬移”
const FADE_WINDOW = 0.6

const scrollY = ref(0)
const vh = ref(window.innerHeight)

// 一整个循环的滚动长度 = 图片数 x 每张图占屏数 x 一屏高
const cycle = computed(() => vh.value * SCROLL_PER_IMAGE * N)
// 背景层保持 2 个循环的高度，便于在滚满一个循环后无缝回绕
const backHeight = computed(() => vh.value * SCROLL_PER_IMAGE * N * 2)

// 每滚 SCROLL_PER_IMAGE 屏切一张图；a 为当前图，b 为下一页要淡入的图，t 为过渡进度 0~1
function layerOpacity(i: number): number {
  const progress = scrollY.value / (vh.value || 1)
  const seg = progress / SCROLL_PER_IMAGE // 当前处于第几段图片
  const page = Math.floor(seg)
  const a = ((page % N) + N) % N // 当前图（正数取模，最后一张回滚到第一张）
  const b = (a + 1) % N // 下一页要淡入的图
  const frac = seg - page // 当前段内的进度 0~1
  // 前 (1-FADE_WINDOW) 完全停留显示，最后 FADE_WINDOW 才渐变到下一张
  const t = Math.min(Math.max((frac - (1 - FADE_WINDOW)) / FADE_WINDOW, 0), 1)
  if (i === a) return 1 - t
  if (i === b) return t
  return 0
}

function onScroll() {
  const current = window.scrollY
  // 滚满一个循环后，回绕一整个周期（内容不动、背景按取模计算同一画面，因此无跳变）
  if (current >= cycle.value) {
    window.scrollTo(0, current - cycle.value)
    scrollY.value = current - cycle.value
  } else {
    scrollY.value = current
  }
}

function onResize() {
  vh.value = window.innerHeight
}

onMounted(() => {
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('resize', onResize)
  onScroll()
})
onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', onResize)
})

//基本个人信息
const name = 'muyangren'
const bio = '写代码，写博客，写生活。'

// 社交链接
const socialLinks = [
  { name: 'GitHub', url: 'https://github.com/muyangrendequzhang' },
  { name: '博客', url: 'https://blog.csdn.net/2401_86822182' },
]

// 文章列表
const posts = [
  { id: 1, title: '我的第一篇博客', date: '2026-08-28', link: '/posts/1' },
  { id: 2, title: 'Vue 3 入门指南', date: '2026-08-20', link: '/posts/2' },
]

// 掌握的技术栈
const skills = ['Vue', 'TypeScript', 'Vite', 'Node.js', 'java', 'SpringBoot']
</script>

<style scoped>
/* 背景层容器：高度由 JS 动态设为 2 个循环，保证能一直滚（配合回绕实现无限循环） */
.back {
  position: relative;
  z-index: 0;
}

/* 每张背景层固定在视口、铺满一屏，用 opacity 实现丝滑淡入淡出 */
.bg-layer {
  position: fixed;
  inset: 0;
  background-size: cover;
  background-position: center;
  z-index: -1;
  will-change: opacity;
}

/* 卡片始终固定在屏幕中央，不随滚动移动 */
.viewport {
  position: sticky;
  top: 0;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: auto;
}

.blog-card {
  background: rgba(237, 207, 237, 0.5); /* 半透明背景 */
  max-width: 640px;
  width: calc(100% - 32px);
  padding: 30px;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  display: block;
  margin-bottom: 16px;
}

h1 {
  margin: 0 0 4px 0;
  font-size: 24px;
}

.bio {
  color: #666;
  margin: 0 0 20px 0;
}

.social-links {
  display: flex;
  gap: 12px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.social-links a {
  color: #2c3e50;
  text-decoration: none;
  padding: 4px 12px;
  border-radius: 20px;
  background: #f0f2f5;
  font-size: 14px;
  transition: background 0.2s;
}

.social-links a:hover {
  background: #e0e4ea;
}

.posts h2,
.skills h2 {
  font-size: 18px;
  margin: 24px 0 12px 0;
}

.posts ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.posts li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #f0f0f0;
}

.posts li a {
  color: #2c3e50;
  text-decoration: none;
}

.posts li a:hover {
  color: #42b883;
  text-decoration: underline;
}

.posts .date {
  font-size: 13px;
  color: #999;
}

.skill-tag {
  display: inline-block;
  padding: 4px 14px;
  margin: 4px 6px 4px 0;
  background: #e8f5e9;
  color: #2e7d32;
  border-radius: 20px;
  font-size: 13px;
}
</style>
