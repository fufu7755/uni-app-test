<template>
	<view>
    <h3 class="view-container">人类列表</h3>
    <uni-list class="bots-list">
      <uni-list-item v-for="(item, index) in bots" :key="index" :title = "item.userName" @click="goTalk(item)" link>
      </uni-list-item>
    </uni-list>
    <button @click="goTalk"></button>
  </view>
</template>

<script>
	import axios from 'axios';
	import config from '@/config.js';
	const baseUrl = config.baseUrl;
	
	export default {
		data() {
			return {
				bots: []
			}
		},
		mounted () {
			this.getBots()
		},
		methods: {
			goTalk (item) {
				uni.navigateTo({
				  url: '/pages/h-h/h-h?uid=' + item._id
				});
			},
			async getBots() {
			  try {
			    const res = await axios.get(baseUrl + '/api/human/list')
			    if (res.data.success) {
					console.log(res.data.data)
					this.bots = res.data.data
			    } else {
					console.log(res.data.err)
			    }        
			  } catch (error) {
					// Handle errors
					console.error('Login error:', error);
			  }
			}
		}
	}
</script>

<style scoped>
.view-container {
  padding: 20px;
}
.bots-list {
	margin-bottom: 30px;
}
</style>
