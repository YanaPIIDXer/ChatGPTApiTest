<script setup lang="ts">
import type { Message } from './interfaces/Message';

const dummy: Message[] = [
  { speaker: "user", text: "USERです" },
  { speaker: "bot", text: "BOTです" },
]
</script>

<template lang="pug">
.root
  header
    h1 ChatGPT
  main
    .inputArea
      textarea.prompt(placeholder="プロンプトを入力してください")
    .chatArea
      template(v-for=("(item, index) in dummy") :key="index")
        .message(:class="{ 'user': item.speaker === 'user', 'bot': item.speaker === 'bot' }") {{ item.text }}
</template>

<style lang="sass" scoped>
.root
  .inputArea
    .prompt
      resize: none
      width: 350px
      height: 50px
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
