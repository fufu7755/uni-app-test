<!-- pages/askAI/askAI.vue -->

<template>
  <view>
    <textarea v-model="question" placeholder="请输入你的问题"></textarea>
    <button @click="askAI">询问AI</button>

    <view class="answer">
      <text>AI回答：</text>
      <text v-if="answer">{{ answer }}</text>
      <text v-else>等待AI回答...</text>
    </view>
  </view>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      question: '',
      answer: ''
    };
  },
  methods: {
    async askAI() {
      try{
        const res = await axios({
          url: 'http://ai.nodefu.net/api/ai',
          method: 'POST',
          data: {
            question: this.question
          }
        })
        console.log('问题', res.data)
        if (res.data.success) {
          this.answer = res.data.data;
        } else {
          this.answer = ''
        }
      }catch(e){
        console.log(e)
      }
    }
  }
};
</script>

<style scoped>
textarea {
  width: 100%;
  height: 100px;
  padding: 10px;
}

button {
  margin-top: 10px;
}

.answer {
  margin-top: 20px;
}
</style>
