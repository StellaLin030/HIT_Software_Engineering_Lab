<template>
  <div class="chat-history">
<!--    <div class="header">-->
<!--      <h2>聊天记录</h2>-->
<!--    </div>-->
    <div class="llm-chat-demo">
      <span class="chat-demo">ChatWIthAIs</span><span class="version"> V1</span>
    </div>

    <div class="user-actions">
      <button @click="goToLogout" class="action-button logout-button">登出</button>
      <button @click="goToFeedback" class="action-button feedback-button">反馈</button>
    </div>

    <div class="messages" ref="messageContainer">
      <div class="container">
        <div class="left">
          <div v-for="(message, index) in wenxin_messages" class="main-message" :key="index"
               :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
            <div class="message-sender"
                 :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
              <img v-if="message.sender === 'user'" src="@/assets/我的.png" alt="User Icon">
              <img v-else-if="message.sender === 'friend'" src="@/assets/文心一言.png" alt="Friend Icon">
              <span class="message-sender-name"
                    :class="message.sender === 'user' ? 'user-color' : 'friend-color'">{{ message.sender }}:</span>
            </div>
            <div v-if="message.sender === 'user'" class="user-message">{{ message.content }}</div>
            <div v-else class="friend-message" v-html="message.content"></div>
          </div>
        </div>
        <div class="mid">
          <div v-for="(message, index) in tongyi_messages" class="main-message" :key="index"
               :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
            <div class="message-sender"
                 :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
              <img v-if="message.sender === 'user'" src="@/assets/我的.png" alt="User Icon">
              <img v-else-if="message.sender === 'friend'" src="@/assets/通义千问.png" alt="Friend Icon">
              <span class="message-sender-name"
                    :class="message.sender === 'user' ? 'user-color' : 'friend-color'">{{ message.sender }}:</span>
            </div>
            <div v-if="message.sender === 'user'" class="user-message">{{ message.content }}</div>
            <div v-else class="friend-message" v-html="message.content"></div>
          </div>
        </div>
        <div class="right">
          <div v-for="(message, index) in chatgpt_messages" class="main-message" :key="index"
               :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
            <div class="message-sender"
                 :class="{'user-message': message.sender === 'user', 'friend-message': message.sender === 'friend'}">
              <img v-if="message.sender === 'user'" src="@/assets/我的.png" alt="User Icon">
              <img v-else-if="message.sender === 'friend'" src="@/assets/chatgpt.png" alt="Friend Icon">
              <span class="message-sender-name"
                    :class="message.sender === 'user' ? 'user-color' : 'friend-color'">{{ message.sender }}:</span>
            </div>
            <div v-if="message.sender === 'user'" class="user-message">{{ message.content }}</div>
            <div v-else class="friend-message" v-html="message.content"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ChatHistory',
  data() {
    return {
      chatgpt_messages: [],
      wenxin_messages: [],
      tongyi_messages: [],
    }
  },
  async mounted() {
    try {
      const conversationId = this.$route.params.id; // 从路由参数中获取对话ID
      const response = await axios.get(`/conversations/get_conversation?id=${conversationId}`);
      if (response.data) {
        this.chatgpt_messages = JSON.parse(response.data.chatgpt_messages);
        this.wenxin_messages = JSON.parse(response.data.wenxin_messages);
        this.tongyi_messages = JSON.parse(response.data.tongyi_messages);
      }
    } catch (error) {
      console.error('Error fetching conversation:', error);
    }
  },
  methods: {
    scrollToBottom() {
      const messageContainer = this.$refs.messageContainer;
      if (messageContainer) {
        messageContainer.scrollTop = messageContainer.scrollHeight;
      }
    }
  }
}
</script>

<style scoped>
.chat-history {
  width: 100%;
  height: 100%;
}

.header {
  padding: 20px;
  background-color: #f5f5f5;
  text-align: center;
}

.right-version {
  margin: auto;
  height: 5%;
  display: flex;
  justify-content: start;
  align-items: center;
  border-radius: 15px;
  margin-bottom: 12px;
}

.version {
  color: rgb(155, 155, 155);
}

.llm-chat-demo {
  width: 58%;
  margin-left: 20px;
  font-family: Söhne, ui-sans-serif, system;
  font-variation-settings: normal;
  font-weight: 550;
  font-size: 18px;
  cursor: pointer;
  color-scheme: light;
}

.chat-demo {
  opacity: 0.65; /* 设置透明度为 0.7，您可以根据需要调整这个值 */
}


.logout-button,
.feedback-button {
  background-color: #1296db;
  color: #fff;
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin-right: 10px;
}

.logout-button:hover,
.feedback-button:hover {
  background-color: #0056b3;
}

.user-actions {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  position: absolute;
  top: 10px; /* 向下移动 */
  right: 10px;
}

.messages {
  height: calc(100% - 60px); /* 调整为合适的高度 */
  overflow-y: auto;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.left {
  grid-column: 1 / 2;
}

.mid {
  grid-column: 2 / 3;
}

.right {
  grid-column: 3 / 4;
}

.main-message {
  margin: auto;
  width: 58%;
  justify-content: center;
}

.message-sender {
  display: flex;
  align-items: center;
}

.message-sender-name {
  margin-left: 10px;
  font-family: Söhne, ui-sans-serif, system;
  font-weight: 550;
  font-size: 18px;
}

.user-message {
  text-align: left;
  padding: 5px;
  margin-bottom: 5px;
  border-radius: 15px;
}

.friend-message {
  background-color: rgba(227, 255, 255, 0.2);
  text-align: left;
  padding: 5px;
  margin-bottom: 5px;
}

.user-color {
  color: #1296db;
}

.friend-color {
  color: #77FC5D;
}
</style>
