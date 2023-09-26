<template>
	<view>
	    <uni-list class="bots-list">
			  <uni-list-item v-for="(item, index) in bots" :key="index" :title = "item.userName">
			  </uni-list-item>
	    </uni-list>
		<button @click="goTalk">开始聊天</button>
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
			console.log('222')
			this.getBots()
		},
		methods: {
			goTalk () {
				uni.navigateTo({
				  url: '/pages/talks/talks'
				});
			},
			async getBots() {
			  try {
			    const res = await axios.get(baseUrl + '/api/bots/list')
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
.bots-list {
	margin-bottom: 30px;
}
</style>
