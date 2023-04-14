<script setup lang="ts">
import { ref, type Ref } from "vue";
import type { Message } from './interfaces/Message';

const messages: Ref<Message[]> = ref([]);
const inputPrompt = ref("");

/**
 * 送信
 */
const onSend = async () => {
  messages.value.push({ speaker: "user", text: inputPrompt.value });
  // TODO: 送信処理実装
  inputPrompt.value = "";
}
</script>

<template lang="pug">
.root
  header
    h1 ChatGPT
  main
    .inputArea
      textarea.prompt(v-model="inputPrompt" placeholder="プロンプトを入力してください")
      button.sendButton(@click="onSend" :disabled="!inputPrompt") 送信
    .chatArea
      template(v-for=("(item, index) in messages") :key="index")
        .message(:class="{ 'user': item.speaker === 'user', 'bot': item.speaker === 'bot' }") {{ item.text }}
</template>

<style lang="sass" scoped>
.root
  .inputArea
    .prompt
      resize: none
      width: 350px
      height: 50px
    .sendButton
      display: block
      width: 120px
      height: 40px
  .chatArea
    width: 300px
    .message
      
      &.user
        text-align: left
        background-color: #0f0
      &.bot
        text-align: right
        background-color: #F22
</style>
