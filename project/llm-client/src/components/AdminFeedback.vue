<template>
  <div>
    <h2>Admin Feedbacks</h2>
    <div v-if="feedbacks.length > 0">
      <ul>
        <li v-for="feedback in feedbacks" :key="feedback.id">
          <p><strong>FEEDBACK ID:</strong> {{ feedback.id }}</p>
          <p><strong>User ID:</strong> {{ feedback.user_id }}</p>
          <p><strong>User NAME:</strong> {{ feedback.username }}</p>
          <p><strong>Message:</strong> {{ feedback.message }}</p>
          <p><strong>Timestamp:</strong> {{ new Date(feedback.timestamp).toLocaleString() }}</p>
        </li>
      </ul>
    </div>
    <div v-else>
      <p>No feedbacks found.</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'AdminFeedback',
  data() {
    return {
      feedbacks: []
    };
  },
  async created() {
    await this.fetchFeedbacks();
  },
  methods: {
    async fetchFeedbacks() {
      try {
        const response = await axios.get('/api/admin/feedback', { withCredentials: true });
        this.feedbacks = response.data.feedbacks;
      } catch (error) {
        console.error('Error fetching feedbacks:', error);
      }
    }
  }
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 20px;
  background-color: #f9f9f9;
  padding: 10px;
  border-radius: 8px;
}
</style>
