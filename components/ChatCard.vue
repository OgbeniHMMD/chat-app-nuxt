<!-- Please remove this file from your project -->
<template>
  <div class="flex flex-col w-full">
    <div v-if="isMe(chat.id)" class="bg-white rounded-lg bg-green-100 p-4 w-2/3 self-end">
      <div class="pb-2">
        {{chat.text }}
      </div>

      <div class="text-xs text-gray-500">
        {{ new Date(chat.time).toLocaleDateString() }} at
        {{ new Date(chat.time).toLocaleTimeString() }}
      </div>
    </div>

    <div v-else class="bg-white rounded-lg bg-green-50 p-4 w-2/3">
      {{ chat.text }}
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  chat: {
    id: Number,
    text: String,
    time: String
  }
}>()

const ID = 419 // hardcoded ID
const isMe = (id: Number) => id === ID

</script>
