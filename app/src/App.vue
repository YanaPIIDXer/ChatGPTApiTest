<script setup lang="ts">
</script>

<template lang="pug">
.root
  header
    h1 ChatGPT
  main
    textarea.prompt(placeholder="プロンプトを入力してください")
</template>

<style lang="sass" scoped>
.root
  .prompt
    resize: none
    width: 350px
    height: 50px
</style>
