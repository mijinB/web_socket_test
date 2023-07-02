<template>
  <div>
    <h1>WebSocket Example</h1>
    <div v-if="connected">
      <p>WebSocket connected!</p>
      <button @click="sendMessage('hello Websocket')">Send Message</button>
      <button @click="onClose">연결끊기</button>
      <ul>
        <li v-for="(message, index) in messages" :key="index">
          {{ message }}
        </li>
      </ul>
    </div>
    <div v-else>
      <button @click="onOpen">연결하기</button>
      <p>WebSocket connecting...</p>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'

  const ws = ref(null);
  const connected = ref(false);
  const messages = ref([]);

  const onOpen = () => {
    if (!connected.value) {
      ws.value = new WebSocket("ws://localhost:8080");
  
      ws.value.onopen = () => {
        connected.value = true;
      }
  
      ws.value.onmessage = (event) => {
        messages.value.push(event.data);
      }
  
      ws.value.onclose = () => {
        console.log("연결 끊기");
        connected.value = false;
        ws.value = null;
        messages.value = [];
      }
    }
  }
  
  const onClose = () => ws.value.close();

  const sendMessage = message => ws.value.send(message);

</script>

<style lang="scss" scoped>

</style>