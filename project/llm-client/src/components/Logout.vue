<template>
  <div>
    <h2>Logging out...</h2>
    <p>{{ logoutMessage }}</p>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.withCredentials = true;
// 设置默认的基础 URL
//axios.defaults.baseURL = 'http://127.0.0.1:5000';
import router from '../router';

export default {
  name: 'LogoutComponent',
  data() {
    return {
      logoutMessage: ''
    };
  },
  async created() {
    await this.logout();
  },
  methods: {
    async logout() {
      try {
        const response = await axios.get('/api/logout', { withCredentials: true });
        console.log('Logout response:', response);
        if (response.data.status === 'success') {
          this.logoutMessage = response.data.message;
          setTimeout(() => {
            router.push({ name: 'Home' });
            this.logoutMessage = '';
          }, 2000);
        } else {
          this.logoutMessage = response.data.message;
        }
      } catch (error) {
        console.error('Logout request failed:', error);
        this.logoutMessage = '登出失败,你还没有登录或者网络有问题！';
      }
    }
  }
};
</script>

<style scoped>
div {
  text-align: center;
  margin-top: 50px;
}
</style>
