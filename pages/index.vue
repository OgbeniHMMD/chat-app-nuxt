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

      <div class="bg-white border-t flex font-medium font-mono border-green-500 text-lg text-center p-2 gap-2">
        <input
          v-model.trim="message"
          placeholder="Enter Message"
          class="bg-transparent w-full py-2 px-4 focus:outline-0"
          @keypress.enter="sendChat"
        >

        <button class="bg-transparent text-green-700 hover:text-green-800" @click="sendChat">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            class="h-8 w-8 bi bi-send-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855H.766l-.452.18a.5.5 0 0 0-.082.887l.41.26.001.002 4.995 3.178 3.178 4.995.002.002.26.41a.5.5 0 0 0 .886-.083l6-15Zm-1.833 1.89L6.637 10.07l-.215-.338a.5.5 0 0 0-.154-.154l-.338-.215 7.494-7.494 1.178-.471-.47 1.178Z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUpdated, onUnmounted } from 'vue'

const chats = ref([])
const message = ref('')
const timer = ref('')

const bottom = ref(null)
const getId = () => sessionStorage.getItem('id')
const randomColor = () => `hsla(${~~(360 * Math.random())},70%,80%,0.6)`

const getColor = () => {
  const color = sessionStorage.getItem('color')
  return color || randomColor()
}

const sendChat = () => {
  if (message.value) {
    chats.value.push({
      id: getId(),
      time: new Date(),
      text: message.value,
      color: getColor()
    })

    message.value = ''
    localStorage.setItem('chats', JSON.stringify(chats.value))
  }
}

onMounted(() => {
  sessionStorage.setItem('id', getId() || crypto.randomUUID().slice(-5))
  sessionStorage.setItem('color', randomColor())

  // Watch localStorage
  timer.value = setInterval(() => {
    chats.value = JSON.parse(localStorage.getItem('chats')) || []
  }, 1000)
})

onUpdated(() => {
  bottom.value?.scrollIntoView({ behavior: 'smooth' })
})

onUnmounted(() => cleanInterval(timer.value))
</script>
