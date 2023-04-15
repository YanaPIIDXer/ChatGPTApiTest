<script setup lang="ts">
import { ref, type Ref } from "vue";
import type { Message } from './interfaces/Message';
import { ChatCompletionRequestMessage, Configuration, OpenAIApi } from "openai";

const messages: Ref<Message[]> = ref([]);
const inputPrompt = ref("");

const configuration = new Configuration({
  apiKey: import.meta.env.VITE_CHATGPT_TOKEN,
});
delete configuration.baseOptions.headers['User-Agent'];
const openai = new OpenAIApi(configuration);

/**
 * 送信
 */
const onSend = async () => {
  const msgs = messages.value.map(m => {
    return {
      role: m.speaker === "user" ? "user" : "assistant",
      content: m.text,
    }
  }) as ChatCompletionRequestMessage[];

  const text = inputPrompt.value;
  inputPrompt.value = "";
  messages.value.push({ speaker: "user", text });

  try {
    const response = await openai.createChatCompletion({
      model: "gpt-3.5-turbo-0301",
      messages: [...msgs, { role: "user", content: text }],
    });
    messages.value.push({ speaker: "bot", text: response.data.choices[0].message?.content ?? "Invalid content." })
  } catch (error) {
    alert("エラー");
    console.error(error);
  }
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
