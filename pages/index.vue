<template>
  <div class="bg-gray-100">
    <div class="container flex flex-col h-screen mx-auto max-w-screen-md p-4 justify-between">
      <div class="text-center pb-4">
        <h1 class="font-medium text-lg pb-2">
          SIMPLE CHAT
        </h1>
        <p class="font-mono text-sm text-gray-600">
          Hint: <a href="/" target="_blank" class="underline">Open this app in another tab</a> to start a new session
        </p>
      </div>

      <div class="bg-white flex flex-col flex-grow py-8 px-4 gap-4 overflow-y-auto">
        <div v-if="!chats.length" class="flex h-full text-center text-gray-700 justify-center items-center">
          Nothing to show! <br>Be the first to send your message.
        </div>

        <div v-for="(chat,i) of chats" :key="i">
          <ChatCard :chat="chat" />
        </div>
        <div ref="bottom" />
      </div>

      <ChatForm />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUpdated, onUnmounted } from 'vue'

const chats = ref([])
const timer = ref('')

const bottom = ref(null)
const getId = () => sessionStorage.getItem('id')

onMounted(() => {
  chats.value = JSON.parse(localStorage.getItem('chats')) || []
  sessionStorage.setItem('id', getId() || crypto.randomUUID().slice(-5))

  // Watch localStorage
  timer.value = setInterval(() => {
    const newValue = JSON.parse(localStorage.getItem('chats')) || []
    if (chats.value.length !== newValue.length) { chats.value = newValue }
  }, 1000)
})

onUpdated(() => {
  bottom.value?.scrollIntoView({ behavior: 'smooth' })
})

onUnmounted(() => cleanInterval(timer.value))
</script>
