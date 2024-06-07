<template>
  <div class="feedback-form">
    <h2>用户反馈</h2>
    <form @submit.prevent="submitFeedback">
      <textarea v-model="feedback" placeholder="请输入您的反馈..." required></textarea>
      <button type="submit">提交反馈</button>
    </form>
    <p>{{ feedbackMessage }}</p>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.withCredentials = true;
export default {
  name: 'FeedbackComponent',
  data() {
    return {
      feedback: '',
      feedbackMessage: ''
    };
  },
  methods: {
    async submitFeedback() {
  try {
    await axios.post('/api/user/feedback', {
      message: this.feedback // 使用正确的字段名
    },{ withCredentials: true });
    this.feedbackMessage = '反馈提交成功！感谢您的反馈。';
    this.feedback = ''; // 清空反馈
  } catch (error) {
    this.feedbackMessage = '反馈提交失败，请稍后重试。';
    console.error('Feedback submission error:', error);
  }
}
  }
};
</script>

<style scoped>
.feedback-form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: auto;
}

textarea {
  height: 100px;
  margin-bottom: 10px;
  resize: vertical;
}

button {
  cursor: pointer;
}
</style>
