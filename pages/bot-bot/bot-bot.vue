<template>
	<view class="chat-container">
		<h3>Bot to Bot</h3>
		<view v-for="(message, index) in talk" :key="index" class="message-item"
			:class="{ 'sent-message': index % 2 === 0, 'received-message': index% 2 === 1 }">
			<view class="message-text">{{ message }}</view>
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
						type: 'q',
						timestamp: '12:30 PM'
					},
					{
						text: 'Hi there!',
						type: 'a',
						timestamp: '12:32 PM'
					},
					{
						text: 'How are you?',
						type: 'q',
						timestamp: '12:33 PM'
					},
					{
						text: 'I\'m good, thanks!',
						type: 'a',
						timestamp: '12:35 PM'
					},
				],
				newMessage: '',
				bots: [],
				talk: []
			};
		},
		async mounted() {
			const talk = await axios({
				method: 'POST',
				url: baseUrl + '/api/bot/talk',
				data: { "name_q":"陈书金", 
					"name_a":"张小小",
					"user_profile_name_q":"【爱好，职业，学历，缺点，讨厌，喜欢】足球球迷，主队曼联;西安人;伦敦生活;很能吃;帝国理工数学本科;UCL科学哲学硕士;骑摩托;喜欢听摇滚;INFP;慢热;好奇心强;容易分心;做事瞻前顾后;做事拖延;不喜欢和陌生人社交;不喜欢过分精致的东西和地方，;不擅长做场面上的事,比如敬酒;在找科学哲学科学史的phd机会;不喜欢太辣的食物;不喜欢潮湿闷热的天气",
					"user_profile_name_a":"【爱好，职业，学历，缺点，讨厌，喜欢】排球球迷，没有主队;海南人;生活在上海,轻度厌食症患者;上海大学佛教研究所;中国政法大学大学本科;喜欢历史;喜欢听英文Rap;性格急躁;对除了历史学和经济学以外的东西毫无兴趣;注意力集中;做事果断;做事毫不拖延;非常喜欢和陌生人社交;喜欢精致的小附件和仪式;擅长社交,比如朝觐圣地;在找宗教历史的phd机会;喜欢甜食和高热量食物;非常喜欢潮湿闷热的天气",
					"debug_mode":true 
					} 
			})
      if (talk.data.success) {
        this.talk = talk.data.data.split('|')
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