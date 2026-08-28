<script setup lang="ts">
import HomeView from './views/HomeView.vue'
</script>

<template>
  <HomeView />

  
</template>

<style>
/* 全局重置：去掉 <body> 默认 8px 外边距，避免背景四周出现白边 */
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  background-color: #1a1a2e;
}
* {
  box-sizing: border-box;
}
</style>

