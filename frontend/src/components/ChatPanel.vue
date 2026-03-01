<template>
  <aside class="panel" :class="{ open: isOpen }" aria-label="Chat panel">
    <div class="panel-header">
      <div class="title">Chats</div>
      <button class="close-btn" type="button" @click="$emit('close')">✕</button>
    </div>

    <div class="list">
      <button v-for="u in users" :key="u.id" class="user" type="button">
        <span class="dot" :class="{ online: u.online }" aria-hidden="true"></span>
        <span class="name">{{ u.name }}</span>
      </button>
    </div>
  </aside>
</template>

<script setup>
defineProps({
  isOpen: { type: Boolean, required: true },
});

/*
  Temporary mock users.
  Later these will come from backend / websocket presence.
*/
const users = [
  { id: "u1", name: "Alex", online: true },
  { id: "u2", name: "Maria", online: false },
  { id: "u3", name: "Nikos", online: true },
  { id: "u4", name: "Eleni", online: false },
];

defineEmits(["close"]);
</script>

<style scoped>
.panel {
  position: fixed;
  top: 56px; /* below the TopBar */
  right: 0;
  height: calc(100vh - 56px);
  width: 320px;
  background: rgba(2, 6, 23, 0.96);
  border-left: 1px solid rgba(148, 163, 184, 0.2);
  transform: translateX(100%);
  transition: transform 160ms ease;
  z-index: 40;
  display: flex;
  flex-direction: column;
}

.panel.open {
  transform: translateX(0);
}

.panel-header {
  height: 52px;
  padding: 0 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid rgba(148, 163, 184, 0.2);
}

.title {
  font-weight: 800;
}

.close-btn {
  width: 32px;
  height: 32px;
  border-radius: 10px;
  border: 0;
  background: rgba(226, 232, 240, 0.08);
  color: #e2e8f0;
  cursor: pointer;
}

.close-btn:hover {
  background: rgba(226, 232, 240, 0.14);
}

.list {
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 8px;
  overflow: auto;
}

.user {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-radius: 12px;
  border: 1px solid rgba(148, 163, 184, 0.2);
  background: rgba(226, 232, 240, 0.06);
  color: #e2e8f0;
  cursor: pointer;
  text-align: left;
}

.user:hover {
  background: rgba(226, 232, 240, 0.1);
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 999px;
  background: rgba(148, 163, 184, 0.7);
}

.dot.online {
  background: #22c55e;
}

.name {
  font-weight: 600;
}
</style>