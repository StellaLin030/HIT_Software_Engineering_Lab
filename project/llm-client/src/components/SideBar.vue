<template>
  <div>
    <div class="sidebar">
      <button class="new-chat-button">新聊天</button>
      <div class="conversation-list">
        <div class="conversation" v-for="(conversation, index) in conversations" :key="index" @click="goToConversation(conversation)">
          {{ conversation.summary }}
        </div>
      </div>
    </div>
    <div class="user-dashboard">
      <router-view></router-view>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: 'SideBar',
  data() {
    return {
      conversations: []
    }
  },
  mounted() {
    this.fetchConversations();
  },
  methods: {
    async fetchConversations() {
      try {
        const response = await axios.get('/api/conversations/get_conversation_summary');
        this.conversations = response.data;
      } catch (error) {
        console.error('Error fetching conversations:', error);
      }
    },
    goToConversation(conversation) {
      // 使用 Vue Router 的动态路由
      this.$router.push({ name: 'ChatHistory', params: { id: conversation.id } });
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 20%;
  padding: 20px;
  background-color: #f5f5f5;
}

.new-chat-button {
  background-color: #42b983;
  color: #fff;
  padding: 10px;
  width: 100%;
  font-weight: bold;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  margin-bottom: 20px;
  cursor: pointer;
}

.conversation-list {
  overflow-y: auto;
  height: calc(100% - 60px); /* 留出按钮和底部的空间 */
}

.conversation {
  padding: 10px;
  background-color: #fff;
  margin-bottom: 10px;
  cursor: pointer;
}

.conversation:hover {
  background-color: #f0f0f0;
}

.user-dashboard {
  width: 80%; /* 右侧宽度 */
  padding: 20px;
}
</style>
