<!-- Please remove this file from your project -->
<template>
  <div class="flex flex-col w-full">
    <div
      :class="(isMe(chat.id) ? 'bg-green-100 self-end' :'bg-gray-50') + ' rounded-lg p-4 w-2/3 '"
      :style="'background-color:  '+ chat.color"
    >
      <div class="flex text-xs text-gray-600 justify-between">
        <div class="">
          <strong>@{{ chat.id }}</strong>
        </div>
        <div class="">
          {{ new Date(chat.time).toLocaleDateString() }} at
          {{ new Date(chat.time).toLocaleTimeString() }}
        </div>
      </div>

      <div class="py-2">
        {{ chat.text }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  chat: {
    id: string,
    text: string,
    time: string
    color: string
  }
}>()

const isMe = (id: string) => id === sessionStorage.getItem('id')
</script>
