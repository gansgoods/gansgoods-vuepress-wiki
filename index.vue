<template>
  <div class="home">
    <h1>这是首页</h1>
    <router-link to="/学习路线/">进入 README</router-link>
    <!-- 如果你的README.md位于文档的根目录下，通常跳转路径可以简化为'/' -->
    <!-- <router-link to="/">进入 README</router-link> -->
  </div>
</template>

<script>
export default {
  name: 'CustomHome',
}
</script>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

h1 {
  margin-bottom: 20px;
}
</style>
