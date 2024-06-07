<template>
  <div>
    <h2>Admin Dashboard</h2>
    <button @click="goToLogout">登出</button>
    <button @click="goToFeedback">查看反馈信息</button>
    <button @click="goToUserInformation">查看用户信息</button> <!-- 添加一个按钮 -->
    <p>{{ logoutMessage }}</p>
  </div>
</template>

<script>
import router from '../router';

export default {
  name: 'AdminDashboard',
  data() {
    return {
      logoutMessage: ''
    };
  },
  methods: {
    goToLogout() {
      router.push({ name: 'Logout' });
    },
    goToFeedback() {
      router.push({ name: 'AdminFeedback' });
    },
    goToUserInformation() {
      router.push({ name: 'AdminUserInfo' }); // 跳转到查看用户信息的页面
    }
  }
};
</script>

<style scoped>
button {
  margin: 10px;
}
</style>
