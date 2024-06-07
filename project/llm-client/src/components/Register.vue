<template>
  <div class="register-container">
    <h2 class="register-title">注册新账号</h2>
    <form @submit.prevent="register" class="register-form">
      <div class="form-group">
        <label for="username">用户名：</label>
        <input type="text" v-model="username" class="input-field" required>
      </div>
      <div class="form-group">
        <label for="password">密码：</label>
        <input type="password" v-model="password" class="input-field" required>
      </div>
      <button type="submit" class="register-button">注册</button>
    </form>
    <p class="register-message">{{ message }}</p>
    <p>已有账号？ <router-link to="/login">点击这里登录</router-link></p>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.withCredentials = true;
export default {
  name: 'RegisterComponent',
  data() {
    return {
      username: '',
      password: '',
      message: ''
    };
  },
  methods: {
    async register() {
      try {
        const response = await axios.post('/api/register', {
          username: this.username,
          password: this.password
        });
        this.message = response.data.message;
        if (response.status === 201) {
          this.message += ' 2s后自动跳转到登录页面...';
          setTimeout(() => {
            this.$router.push('/login');
          }, 2000); // 2秒后重定向到登录页面
        }
      } catch (error) {
        this.message = error.response && error.response.data ? error.response.data.message : '注册失败，请稍后重试。';
      }
    }
  }
};
</script>

<style scoped>
.register-container {
  max-width: 400px;
  margin: 100px auto;
  text-align: center;
}

.register-title {
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

.register-button {
  background-color: #28a745;
  color: #fff;
  padding: 12px 24px;
  border: none;
  border-radius: 5px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.register-button:hover {
  background-color: #218838;
}

.register-message {
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
