<script lang="ts">
import { ref, onMounted } from 'vue'
import Pusher from 'pusher-js'

interface Message {
  message: string
  username: string
}

export default {
  name: 'App',
  setup() {
    const username = ref<string>('Enter username')
    const messages = ref<Message[]>([])
    const message = ref<string>('')
    const PUSHER_KEY = import.meta.env.VITE_PUSHER_KEY as string

    onMounted(() => {
      Pusher.logToConsole = true

      const pusher = new Pusher(PUSHER_KEY, {
        cluster: 'ap4'
      })

      const channel = pusher.subscribe('chat')
      channel.bind('message', (data: Message) => {
        messages.value.push(data)
      })
    })

    const submit = async () => {
      await fetch('http://localhost:8000/api/messages', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          username: username.value,
          message: message.value
        })
      })

      message.value = ''
    }

    return {
      username,
      messages,
      message,
      submit
    }
  }
}
</script>

<template>
  <div
    class="flex flex-col items-center justify-center w-full h-screen bg-gray-900 py-11 px-11 lg:px-96 gap-4"
  >
    <div class="flex flex-col rounded-xl h-full w-full bg-white">
      <div
        class="flex items-center rounded-t-xl p-6 text-decoration-none bg-gray-300 drop-shadow-md"
      >
        <input class="bg-gray-300 text-gray-900 w-full p-1" v-model="username" />
      </div>
      <div class="overflow-x-hidden m-6 hidden-bar">
        <div class="flex flex-col mt-4" v-for="message in messages" :key="message">
          <div
            class="flex justify-start items-center h-auto p-4 shadow-sm bg-blue-500 text-white rounded-xl break-words whitespace-pre-wrap"
          >
            {{ message.message }}
          </div>
          <div class="flex w-full items-center justify-between ml-3 mt-1">
            <span class="text-xs text-gray-400">{{ message.username }}</span>
          </div>
        </div>
      </div>
    </div>
    <form class="block w-full" @submit.prevent="submit">
      <input
        class="bg-white text-gray-900 rounded-xl w-full min-h-12 h-auto p-6"
        placeholder="Write a message"
        v-model="message"
      />
    </form>
  </div>
</template>

<style>
.hidden-bar {
  -ms-overflow-style: none;
  scrollbar-width: none;
  overflow-y: scroll;
}

.hidden-bar::-webkit-scrollbar {
  display: none;
}
</style>
