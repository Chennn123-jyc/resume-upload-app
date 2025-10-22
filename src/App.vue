<template>
  <div id="app">
    <ResumeUpload />
  </div>
</template>

<script setup>
import ResumeUpload from './components/ResumeUpload.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f5f7fa;
}

#app {
  min-height: 100vh;
}
</style>