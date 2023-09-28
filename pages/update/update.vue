<template>
	<div class="user-info-edit">
		<h3>用户信息修改</h3>
		<div class="input-group">
			<label for="name">Name</label>
			<input id="name" v-model="userInfo.userName" placeholder="Enter your name" />
		</div>

		<div class="input-group">
			<label>Gender</label>
			<radio-group @change="radioChange" class='horizontal-radio'>
				<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in genders" :key="item">
					<view class="list-item">
						<radio :value="item" :checked="selectGender === item" />
					</view>
					<view>{{item}}</view>
				</label>
			</radio-group>
		</div>

		<div class="input-group">
			<label for="birthdate">Birthdate</label>
			<uni-datetime-picker v-model="demosBirthday" type="date" :format="datePickerFormat" placeholder="Select your birthdate"></uni-datetime-picker>
		</div>

		<button class="save-button" @click="saveUserInfo">Save</button>
	</div>
</template>

<script>
import axios from 'axios'
import config from '@/config.js'
import moment from 'moment'
const baseUrl = config.baseUrl;

export default {
	data() {
		return {
			userInfo: {
				userName: '',
				demosGender: '',
				demosBirthday: '' // Format: 'YYYY-MM-DD'
			},
			datePickerFormat: 'YYYY-MM-DD',
			genders: ['M', 'F', 'M/F'],
			selectGender: '',
			demosBirthday: ''
		}
	},
	async mounted () {
		const res = await axios({
			url: baseUrl + '/api/user/info',
			method: 'GET'
		})
		if (res.data.success) {
			this.userInfo = res.data.data
			console.log(this.userInfo)
			this.selectGender = this.userInfo.demosGender
			this.demosBirthday = moment(this.userInfo.demosBirthday).format('YYYY-MM-DD')
		}
	},
	methods: {
		async saveUserInfo() {
			// TODO: Implement save user info logic
			console.log('User info saved:', this.userInfo);
			this.userInfo.demosBirthday = this.demosBirthday
			const res = await axios({
				url: baseUrl + '/api/user/update',
				method: 'POST',
				data: this.userInfo
			})
			if (res.data.success) {
				uni.showToast({
					title: '更新成功！',
					duration: 2000
				});
			}
		},
		
		radioChange(e) {
			console.log(e)
			this.selectGender = e.detail.value
			this.userInfo.demosGender = e.detail.value
		}
	}
}
</script>

<style scoped>
.user-info-edit {
  padding: 40px;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.input-group input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.horizontal-radio {
  display: flex;
  align-items: center;
}

.uni-list-cell .list-item {
  margin-right: 10px;
}

.save-button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #3498db;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.save-button:hover {
  background-color: #2980b9;
}
</style>
