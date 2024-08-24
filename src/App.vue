<template>
  <div class="min-h-screen w-full flex items-center justify-center bg-gray-100">
    <div class="p-6 max-w-sm mx-auto bg-white rounded-lg">
      <h1 class="text-2xl font-semibold mb-4 text-center text-black">
        Realtime Autobots Count
      </h1>
      <div class="text-center text-4xl font-bold text-gray-800">
        {{ count }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const count = ref(0);
let ws;

const setupWebSocket = () => {
  ws = new WebSocket('ws://localhost:3000');

  ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    count.value = data.count;
  };

  ws.onopen = () => {
    console.log('WebSocket connection opened');
  };

  ws.onclose = () => {
    console.log('WebSocket connection closed');
  };

  ws.onerror = (error) => {
    console.error('WebSocket error:', error);
  };
};

onMounted(() => {
  setupWebSocket();
});

onUnmounted(() => {
  if (ws) {
    ws.close();
  }
});
</script>

<style scoped>
/* Scoped styles for your component */
</style>
