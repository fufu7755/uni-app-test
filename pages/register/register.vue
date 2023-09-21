<template>
  <div class="registration-container">
    <h1 class="registration-title">注册</h1>
    <view class="registration-form">
		<div class="form-group">
			<label for="username">电子邮件：</label>
			<input v-model="email" type="text" id="email" class="form-input" required>
		</div>
		<div class="form-group">
			<label for="password">密码：</label>
			<input v-model="password" type="password" id="password" class="form-input" required>
		</div>
		<button type="submit" class="submit-button" @click="register">注册</button>
    </view>
  </div>
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
    async register() {
		console.log('email', this.email)
		const res = await axios({
			url: baseUrl + '/api/user/register',
			method: 'POST',
			data: {
				email: this.email,
				password: this.password
			}
		})
		if (res.data.success) {
			uni.navigateTo({
			  url: '/pages/login/login'
			});
		} else {
			console.log(res.data.err)
		}
    }
  }
};
</script>

<style scoped>
.registration-container {
  max-width: 300px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
}

.registration-title {
  text-align: center;
  margin-bottom: 20px;
}

.registration-form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 15px;
}

.form-input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.submit-button {
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #3498db;
  color: white;
  font-size: 16px;
  cursor: pointer;
  width: 100%;
}

.submit-button:hover {
  background-color: #2980b9;
}
</style>
