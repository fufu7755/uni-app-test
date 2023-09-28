<template>
  <view class="login-page">
    <view class="login-container">
      <button class="btn" @click="userUpdate">用户信息修改</button>
			<button class="btn" @click="botTalk">机器和机器对话</button>
			<button class="btn" @click="humanBot">人和机器对话</button>
      <button @click="login">登录</button>
    </view>
  </view>
</template>

<script>
import axios from 'axios';
import config from '@/config.js';
const baseUrl = config.baseUrl;

export default {
  data() {
    return {
      email: '',
      password: ''
    };
  },
  methods: {
		userUpdate () {
			uni.navigateTo({
				url: '/pages/update/update'
			});
		},
    botTalk () {
			uni.navigateTo({
			  url: '/pages/bot-bot/bot-bot'
			});
		}
  }
};
</script>

<style scoped lang="scss">
.btn {
	margin-bottom: 20px;
}

.login-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f4f4f4;
}

.login-container {
  width: 300px;
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.logo {
  width: 100px;
  height: 100px;
  margin-bottom: 20px;
}

input {
  padding: 12px;
  margin-bottom: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  width: 100%;
  padding: 12px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #2980b9;
}
</style>
