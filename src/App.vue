<script setup>
import construct from './components/construct'
import preview from './components/preview'
import { reactive } from 'vue'

const messages = reactive([])

function onMessagesChange (payload) {
  messages.length = 0
  messages.push(...payload)
}
</script>

<template>
  <construct @messagesChange="onMessagesChange" />
  <preview :messages="messages" />
</template>

<style lang="scss">
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: -apple-system, system-ui, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  font-size: 14px;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px;
  min-height: 100vh;
}

button {
  cursor: pointer;
}
</style>
