<template>
  <div>
    <h2>User Information</h2>
    <div v-if="users.length > 0">
      <ul>
        <li v-for="user in users" :key="user.id">
          <strong>UserID:</strong> {{ user.id }}
          <strong>Username:</strong> {{ user.username }}
          <strong>Is_admin:</strong> {{ user.is_admin }}
          <!-- 其他用户信息 -->
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No users found.</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.withCredentials = true;
export default {
  name: 'UserInformation',
  data() {
    return {
      users: []
    };
  },
  async created() {
    await this.fetchUsers();
  },
  methods: {
    async fetchUsers() {
      try {

        const response = await axios.get('/api/admin/users',{ withCredentials: true });
        this.users = response.data.users;
      } catch (error) {
        console.error('Failed to fetch users:', error);
      }
    }
  }
};
</script>

<style scoped>
/* Your styles here */
</style>
