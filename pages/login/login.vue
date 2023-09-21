<template>
  <view class="login-page">
    <view class="login-container">
      <input v-model="email" type="text" placeholder="请输入邮箱" />
      <input v-model="password" type="password" placeholder="请输入密码" />
      <button @click="login">登录</button>
    </view>
  </view>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: ''
    };
  },
  methods: {
    async login() {
      try {
        const res = await axios.post('http://ai.nodefu.net/api/login', {
          email: this.email,
          password: this.password
        });
        if (res.data.success) {
          console.log('Login successful:', res.data);
          uni.navigateTo({
            url: '/pages/ai/ai'
          });
        } else {
          console.log(res.data.err)
        }        
      } catch (error) {
        // Handle errors
        console.error('Login error:', error);
      }
    }
  }
};
</script>

<style scoped lang="scss">
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
