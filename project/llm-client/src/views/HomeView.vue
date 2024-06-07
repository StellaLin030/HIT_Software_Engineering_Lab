<template>
  <div class="home">
    <h1>欢迎使用大模型一站式访问</h1>
    <router-link to="/login" class="home-link">登录</router-link> |
    <router-link to="/register" class="home-link">注册</router-link>
    <h2>不知道说什么，没有审美，改不出来好看的页面 :(</h2>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
};
</script>

<style scoped>
.home {
  text-align: center;
  margin-top: 50px;
}

.home h1 {
  font-size: 28px;
}

.home-link {
  margin: 0 10px;
  font-size: 16px;
  color: #42b983;
}

.home h2 {
  margin-top: 20px;
  font-size: 18px;
}
</style>
