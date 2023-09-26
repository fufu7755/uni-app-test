<template>
	<view class="chat-container">
		<view v-for="(message, index) in messages" :key="index" class="message-item"
			:class="{ 'sent-message': message.type === 'sent', 'received-message': message.type === 'received' }">
			<view class="message-content">
				<view class="message-text">{{ message.text }}</view>
			</view>
			<view class="timestamp">{{ message.timestamp }}</view>
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
				messages: [{
						text: 'Hello!',
						type: 'received',
						timestamp: '12:30 PM'
					},
					{
						text: 'Hi there!',
						type: 'sent',
						timestamp: '12:32 PM'
					},
					{
						text: 'How are you?',
						type: 'sent',
						timestamp: '12:33 PM'
					},
					{
						text: 'I\'m good, thanks!',
						type: 'received',
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
			// const talk = await axios({
			// 	method: 'POST',
			// 	url: baseUrl + '/api/talk/check',
			// 	data: {
			// 		q: bots[0]._id,
			// 		a: bots[1]._id
			// 	}
			// })
		},
		methods: {
			sendMessage() {
				if (this.newMessage.trim() !== '') {
					this.messages.push({
						text: this.newMessage,
						type: 'sent',
						timestamp: this.getCurrentTimestamp()
					});
					this.newMessage = ''; // Clear the input after sending message
				}
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