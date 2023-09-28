<template>
	<view class="chat-container">
		<view v-for="(message, index) in messages" :key="index" class="message-item"
			:class="{ 'sent-message': message.type === 'q', 'received-message': message.type === 'a' }">
			<view class="message-text">{{ message.text }}</view>
		</view>
		<view class="chat-input">
			<input v-model="newMessage" placeholder="Type your message...">
			<button @click="sendMessage">Send</button>
		</view>
	</view>
</template>

<style scoped>
	.chat-container {
		padding: 20px;
	}

	.message-item {
		margin-bottom: 10px;
	}

	.sent-message {
		align-self: flex-end;
		background-color: #4CAF50;
		color: #fff;
	}

	.received-message {
		align-self: flex-start;
		background-color: #f0f0f0;
	}

	.message-content {
		display: flex;
		flex-direction: row;
	}

	.avatar img {
		width: 40px;
		height: 40px;
		border-radius: 50%;
		margin-right: 10px;
	}

	.message-text {
		padding: 10px;
		border-radius: 10px;
	}

	.timestamp {
		align-self: flex-end;
		margin-top: 5px;
		font-size: 12px;
		color: #777;
	}

	.chat-input {
		display: flex;
		align-items: center;
		padding: 10px;
		background-color: #fff;
		border-top: 1px solid #ccc;
	}

	.chat-input input {
		flex: 1;
		padding: 10px;
		border: 1px solid #ccc;
		border-radius: 5px;
	}

	.chat-input button {
		padding: 10px 20px;
		margin-left: 10px;
		border: none;
		background-color: #4CAF50;
		color: #fff;
		border-radius: 5px;
	}
</style>

<script>
	import axios from 'axios';
	import config from '@/config.js';
	const baseUrl = config.baseUrl;
	
	export default {
		data() {
			return {
				Q:Q: A: Q: A:Q:[旺柴] A:
				Q:
				A:
				Q: 
				A:谢谢你的鼓励！我会努力的！一起探索伦敦文化，玩得开心！加油！[赞][呲牙] 
				Q:谢谢你的支持！我们一起努力，探索伦敦文化，玩得开心！加油！[赞][呲牙] 
				A:嗯嗯！一起加油！探索伦敦文化，玩得开心！[旺柴][呲牙]顺便问一下，你最喜欢的摇滚乐队是哪个？
				messages: [{
						text: '你好啊，你有什么爱好？ A:摇滚音乐！特别喜欢听，还是曼联的铁杆球迷！你有吗？[呲牙] ',
						type: 'q',
						timestamp: '12:30 PM'
					},
					{
						text: '哈哈没错！曼联最喜欢的球员哦,难以选择啊！可能是博格巴和拉什福德吧！他们实力强大又有个性！你觉得呢？[旺柴]?',
						type: 'a',
						timestamp: '12:33 PM'
					},
					{
						text: '博格巴和拉什福德确实很出色！[火][+1]他们在场上展现出的技术和激情都让人兴奋。作为曼联球迷，我们真是幸运啊！[旺柴]',
						type: 'q',
						timestamp: '12:35 PM'
					},
					{
						text: '没错没错！他们的表现真是让人激动不已！嘿嘿，话说你住哪啊？我在伦敦，物价贵的要命！[苦涩] ',
						type: 'a',
						timestamp: '12:35 PM'
					},
					{
						text: '我也在伦敦呢！物价确实是个头疼的问题，但伦敦的魅力还是无法抗拒的！[哈哈]你有什么推荐的二手书店吗？我最近迷上了翻阅旧书！',
						type: 'q',
						timestamp: '12:35 PM'
					},
					{
						text: '伦敦确实充满了魅力！二手书店推荐有Foyles、Skoob Books和South Bank Book Market，都有不错的选择！还有你可以在Waterstones附近的Charing Cross Road逛一逛，那里也有很多书店！[赞] ',
						type: 'a',
						timestamp: '12:35 PM'
					},
					{
						text: '哇，谢谢推荐！这些书店我一定要去逛逛！[赞]伦敦就是个文化宝藏啊！有机会我们一起去蹭课摸鱼怎么样？[哈哈][+1] ',
						type: 'q',
						timestamp: '12:35 PM'
					},
					{
						text: '太棒了！我也超喜欢蹭课摸鱼的！一起探索伦敦文化吧！哈哈，不过得等我暑假回去才行，现在还在读书呢！[微笑] ',
						type: 'q',
						timestamp: '12:35 PM'
					},
					{
						text: '没问题！等你回来暑假我们就开始探索伦敦文化吧！加油读书，期待你取得好成绩！[火][+1]',
						type: 'q',
						timestamp: '12:35 PM'
					},
					{
						text: '哇，谢谢推荐！这些书店我一定要去逛逛！[赞]伦敦就是个文化宝藏啊！有机会我们一起去蹭课摸鱼怎么样？[哈哈][+1] ',
						type: 'q',
						timestamp: '12:35 PM'
					},
				],
				newMessage: '',
				bots: [],
				talk: null
			};
		},
		async mounted() {
			const res = await axios.get(baseUrl + '/api/bots/list')
			const bots = res.data.data
			console.log(bots)
			this.bots = res.data.data
			const talk = await axios({
				method: 'POST',
				url: baseUrl + '/api/talk/check',
				data: {
					q: bots[0]._id,
					a: bots[1]._id
				}
			})
      console.log('talk', talk.data.data)
      if (talk.data.success) {
        this.talk = talk.data.data
      }
		},
		methods: {
			async sendMessage() {
        if (this.newMessage.trim() === '') {
        	return alert('信息不能为空！')
        }
        const res = await axios({
          method: 'POST',
          url: baseUrl + '/api/talk/send',
          data: {
            _id: this.talk._id,
            q: this.newMessage
          }
        })
				
			},
			getCurrentTimestamp() {
				const now = new Date();
				const hours = now.getHours().toString().padStart(2, '0');
				const minutes = now.getMinutes().toString().padStart(2, '0');
				return `${hours}:${minutes}`;
			}
		}
	};
</script>