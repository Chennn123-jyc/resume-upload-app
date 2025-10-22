<template>
  <div id="app">
    <!-- 主应用组件 -->
    <ResumeUpload />
  </div>
</template>

<script setup>
// 导入简历上传组件
import ResumeUpload from './components/ResumeUpload.vue'
</script>

<style>
/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* 基础样式 */
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f5f7fa;
}

/* 应用容器 */
#app {
  min-height: 100vh;
}
</style>