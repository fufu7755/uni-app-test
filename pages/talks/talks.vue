<template>
  <div class="chat-container">
    <div class="chat-messages" ref="chatMessages">
      <div v-for="message in messages" :key="message._id" class="message" :class="{ 'sent': message.userId !== uid, 'received': message.userId === uid }">
        {{message.userName}}:{{ message.text }}
      </div>
    </div>
    <div class="chat-input">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="输入消息..." />
      <button @click="sendMessage">发送</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import config from '@/config.js';
const baseUrl = config.baseUrl;
export default {
  data() {
    return {
      messages: [],
      newMessage: '',
      uid: '',
      tid: ''
    };
  },
  onLoad(options) {
    this.uid = options.uid || '651274bac23864151972fb4f'
    console.log(this.uid)
    this.getTalk()
  },
  methods: {
    async getTalk () {
      const res = await axios({
        url: baseUrl + '/api/talk/check',
        method: 'POST',
        data: {
          uid: this.uid
        }
      })
      console.log(res.data)
      if (res.data.success) {
        this.tid = res.data.data._id
        this.messages = res.data.data.history
      }
    },
    
    async sendMessage() {
      if (this.newMessage.trim() === '') return;
      if (!this.tid) return;
      const res = await axios({
        url: baseUrl + '/api/talk/h-b',
        method: 'POST',
        data: {
          msg: this.newMessage.trim(),
          tid: this.tid
        }
      })
      if (res.data.success) {
        this.messages = res.data.data.history
        this.newMessage = '';
      }
      
      // 滚动到聊天窗口底部
      this.$nextTick(() => {
        this.$refs.chatMessages.scrollTop = this.$refs.chatMessages.scrollHeight;
      });
    }
  }
};
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  display: flex;
  flex-direction: column;
}

.message {
  padding: 10px;
  margin: 5px;
  border-radius: 10px;
  max-width: 70%;
}

.sent {
  background-color: #DCF8C6;
  align-self: flex-end;
}

.received {
  background-color: #E0E0E0;
  align-self: flex-start;
}

.chat-input {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background-color: #f5f5f5;
  position: fixed;
  bottom: 0;
  width: 100%;
  box-sizing: border-box;
}

.chat-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

.chat-input button {
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
}

.chat-input button:hover {
  background-color: #45a049;
}
</style>
