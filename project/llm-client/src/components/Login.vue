<template>
  <div class="login-container">
    <h2 class="login-title">欢迎回来！</h2>
    <form @submit.prevent="login" class="login-form">
      <div class="form-group">
        <label for="username">用户名：</label>
        <input type="text" v-model="username" class="input-field" required>
      </div>
      <div class="form-group">
        <label for="password">密码：</label>
        <input type="password" v-model="password" class="input-field" required>
      </div>
      <button type="submit" class="login-button">登录</button>
    </form>
    <p class="login-message">{{ message }}</p>
    <p>还没有账号？ <router-link to="/register">点击这里注册</router-link></p>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.withCredentials = true;
import router from '@/router/index.js';

export default {
  name: 'LoginComponent',
  data() {
    return {
      username: '',
      password: '',
      message: ''
    };
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('/api/login', {
          username: this.username,
          password: this.password
        }, { withCredentials: true });
        this.message = response.data.message;
        if (response.data.role === 'admin') {
          router.push({ name: 'AdminDashboard' });
        } else {
          router.push({ name: 'UserDashboard' });
        }
      } catch (error) {
        this.message = error.response && error.response.data ? error.response.data.message : '登录失败，请稍后重试。';
      }
    }
  }
};
</script>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 100px auto;
  text-align: center;
}

.login-title {
  margin-bottom: 30px;
  font-size: 28px;
  font-weight: bold;
}

.form-group {
  margin-bottom: 20px;
  display: flex;
  align-items: center;
}

.form-group label {
  width: 80px;
}

.input-field {
  width: 250px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.input-field:focus {
  outline: none;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
}

.login-button {
  background-color: #1296db;
  color: #fff;
  padding: 12px 24px;
  border: none;
  border-radius: 5px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.login-button:hover {
  background-color: #0056b3;
}

.login-message {
  margin-top: 20px;
  font-size: 16px;
  color: #ff0000;
}

a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>
