<template>
    <div class="blog-card">
        <!-- 头像 -->
        <img 
            v-if="avatar" 
            :src="avatar" 
            alt="avatar" 
            class="avatar"
        />

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
            <span 
                v-for="skill in skills" 
                :key="skill"
                class="skill-tag"
            >
                {{ skill }}
            </span>
        </div>
    </div>
</template>

<script lang="ts" setup>
// ==================== 基本信息（改成你自己的） ====================
const name = 'muyangren'
const bio = '写代码，写博客，写生活。'
const avatar = ''  // 图片 URL，不填则不显示

// ==================== 社交链接（改成你自己的） ====================
const socialLinks = [
    { name: 'GitHub', url: 'https://github.com/你的用户名' },
    { name: '掘金', url: 'https://juejin.cn/user/你的ID' },
    { name: '博客', url: 'https://你的博客地址' },
]

// ==================== 文章列表（改成你自己的） ====================
const posts = [
    { id: 1, title: '我的第一篇博客', date: '2026-08-28', link: '/posts/1' },
    { id: 2, title: 'Vue 3 入门指南', date: '2026-08-20', link: '/posts/2' },
]

// ==================== 技术栈（改成你自己的） ====================
const skills = [
    'Vue', 'TypeScript', 'Vite', 'Node.js', 'Python', 'Docker'
]
</script>

<style scoped>
.blog-card {
    max-width: 640px;
    margin: 40px auto;
    padding: 30px;
    background: #fff;
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