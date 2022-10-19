<template>
  <section class="bg-white border-t flex font-medium font-mono border-green-500 text-lg text-center p-2 gap-2">
    <input
      v-model.trim="message"
      placeholder="Enter Message"
      class="bg-transparent w-full py-2 px-4 focus:outline-0"
      @keypress.enter="sendChat"
    >

    <button class="bg-transparent text-green-700 hover:text-green-800" @click="sendChat">
      <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor" class="h-8 w-8 bi bi-send-fill" viewBox="0 0 16 16">
        <path
          d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855H.766l-.452.18a.5.5 0 0 0-.082.887l.41.26.001.002 4.995 3.178 3.178 4.995.002.002.26.41a.5.5 0 0 0 .886-.083l6-15Zm-1.833 1.89L6.637 10.07l-.215-.338a.5.5 0 0 0-.154-.154l-.338-.215 7.494-7.494 1.178-.471-.47 1.178Z"
        />
      </svg>
    </button>
  </section>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const message = ref('')

const getId = () => sessionStorage.getItem('id')
const randomColor = () => `hsla(${~~(360 * Math.random())},70%,80%,0.6)`

const getColor = () => {
  const color = sessionStorage.getItem('color')
  return color || randomColor()
}

const sendChat = () => {
  const chats = JSON.parse(localStorage.getItem('chats')) || []

  if (message.value) {
    chats.push({
      id: getId(),
      time: new Date(),
      text: message.value,
      color: getColor()
    })

    localStorage.setItem('chats', JSON.stringify(chats))
    message.value = ''
  }
}

onMounted(() => {
  if (!sessionStorage.getItem('color')) { sessionStorage.setItem('color', randomColor()) }
})
</script>
