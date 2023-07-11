<script lang="ts">
import { ref, onMounted } from 'vue'
import Pusher from 'pusher-js'

export default {
  name: 'App',
  setup() {
    const username = ref<string>('username')
    const messages = ref<Array<string>>([])
    const message = ref<string>('')
    const PUSHER_KEY = import.meta.env.VITE_PUSHER_KEY

    onMounted(() => {
      Pusher.logToConsole = true

      const pusher = new Pusher(PUSHER_KEY, {
        cluster: 'ap4'
      })

      const channel = pusher.subscribe('chat')
      channel.bind('message', (data: { message: string }) => {
        messages.value.push(data.message)
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
  <div class="flex flex-col items-center w-screen h-screen lg:w-1/2 bg-red-300">
    <div class="flex flex-col bg-blue-300">
      <div class="flex items-center p-6 text-decoration-none border-b">
        <input class="" v-model="username" />
      </div>
      <div class="scrollarea">
        <div class="" v-for="message in messages" :key="message">
          <div class="">{{ message }}</div>
          <div class="flex w-full items-center justify-between">
            <span class="text-xs text-gray-500">{{ username }}</span>
          </div>
        </div>
      </div>
    </div>
    <form @submit.prevent="submit">
      <input
        class="bg-white text-gray-800 rounded"
        placeholder="Write a message"
        v-model="message"
      />
    </form>
  </div>
</template>

<style>
.scrollarea {
  min-height: 500px;
}
</style>
