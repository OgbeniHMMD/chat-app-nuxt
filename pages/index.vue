<template>
  <div class="bg-gray-100">
    <div class="container flex flex-col h-screen mx-auto max-w-screen-md p-4 justify-between">
      <div class="text-center pb-4">
        <h1 class="font-medium text-lg pb-2">
          SIMPLE CHAT
        </h1>
        <p class="font-mono text-sm text-gray-600">
          Hint: Open this app in another tab to open a new session
        </p>
      </div>

      <div class="bg-white flex flex-col flex-grow py-8 px-4 gap-4 overflow-y-auto">
        <div v-if="!chats.length" class="flex h-full text-center text-gray-700 justify-center items-center">
          Nothing to show! <br><br>Be the first to send your message.
        </div>

        <div v-for="(chat,i) of chats" :key="i">
          <ChatCard :chat="chat" />
        </div>
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
import { ref, onMounted } from 'vue'

// constants
const id = 419 // hardcoded ID

// reactive state
const chats = ref([])
const message = ref('')

// functions that mutate state and trigger updates
function sendChat () {
  if (message.value) {
    chats.value.push({
      id,
      time: new Date(),
      text: message.value
    })

    message.value = ''
  }
}

// lifecycle hooks
onMounted(() => {
  const init = [{
    id: 0,
    time: '01/01/2022',
    text: 'Nothing to show. Be the first to send your message.'
  }]

  chats.value = JSON.parse(localStorage.getItem('chats')) || init
})
</script>
