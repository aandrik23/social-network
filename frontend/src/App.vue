<template>
  <TopBar @search="handleSearch" @toggle-chat="toggleChat" />
  <RouterView />

  <ChatPanel :isOpen="isChatOpen" @close="isChatClose = false" />
</template>

<script setup>
import { ref } from "vue";
import TopBar from "./components/TopBar.vue";
import ChatPanel from "./components/ChatPanel.vue";

const isChatOpen = ref(false);

function toggleChat() {
  isChatOpen.value = !isChatOpen.value;
}

function handleSearch(query) {
  console.log("Search:", query);
}
</script>

<style>
/* Global styles for the whole app */
body {
  margin: 0;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, Helvetica, Arial, sans-serif;
  background-color: #0f172a;
  color: #e2e8f0;
}
</style>