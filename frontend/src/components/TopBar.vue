<template>
  <header class="topbar">
    <div class="left">
      <RouterLink class="brand" to="/">Social Network</RouterLink>
    </div>

    <nav class="center" aria-label="Primary">
      <RouterLink class="icon-btn" to="/" title="Home" aria-label="Home">
        <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
          <path
            d="M12 3l9 8h-3v9h-5v-6H11v6H6v-9H3l9-8z"
            fill="currentColor"
          />
        </svg>
      </RouterLink>

      <RouterLink class="icon-btn" to="/profile" title="Profile" aria-label="Profile">
        <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
          <path
            d="M12 12a4.5 4.5 0 1 0-4.5-4.5A4.5 4.5 0 0 0 12 12zm0 2c-4.4 0-8 2.2-8 5v2h16v-2c0-2.8-3.6-5-8-5z"
            fill="currentColor"
          />
        </svg>
      </RouterLink>

      <button
        class="icon-btn"
        type="button"
        title="Search"
        aria-label="Search"
        :aria-expanded="isSearchOpen"
        @click="toggleSearch"
      >
        <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
          <path
            d="M10 2a8 8 0 1 0 5.3 14L20 20.7 21.4 19.3 16.7 14.6A8 8 0 0 0 10 2zm0 2a6 6 0 1 1 0 12 6 6 0 0 1 0-12z"
            fill="currentColor"
          />
        </svg>
      </button>

      <div v-if="isSearchOpen" class="search">
        <input
          ref="searchInput"
          v-model.trim="query"
          class="search-input"
          type="text"
          placeholder="Search profiles..."
          @keydown.enter="submitSearch"
          @keydown.esc="closeSearch"
        />
        <button class="search-go" type="button" @click="submitSearch" :disabled="!query">
          Go
        </button>
      </div>
    </nav>

    <div class="right">
      <button
        class="icon-btn"
        type="button"
        title="Notifications"
        aria-label="Notifications"
        @click="$emit('toggle-notifications')"
      >
        <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
          <path
            d="M12 22a2 2 0 0 0 2-2h-4a2 2 0 0 0 2 2zm6-6V11a6 6 0 1 0-12 0v5L4 18v1h16v-1l-2-2z"
            fill="currentColor"
          />
        </svg>
        <span v-if="notificationsCount > 0" class="badge">{{ notificationsCount }}</span>
      </button>

      <!-- Chat icon: toggles the right chat panel -->
      <button class="icon-btn" type="button" title="Chat" aria-label="Chat" @click="$emit('toggle-chat')">
        <svg viewBox="0 0 24 24" class="icon" aria-hidden="true">
          <path
            d="M4 4h16v11H7l-3 3V4zm2 2v9.2L7.2 13H18V6H6z"
            fill="currentColor"
          />
        </svg>
        <span v-if="messagesCount > 0" class="badge">{{ messagesCount }}</span>
      </button>

      <button class="avatar" type="button" title="Account" aria-label="Account" @click="$emit('open-user-menu')">
        <span class="avatar-dot" aria-hidden="true"></span>
      </button>
    </div>
  </header>
</template>

<script setup>
import { nextTick, ref } from "vue";

defineEmits(["toggle-notifications", "open-user-menu", "search", "toggle-chat"]);

const isSearchOpen = ref(false);
const query = ref("");
const searchInput = ref(null);

/*
  Temporary mock counts.
  Later these will come from a store (Pinia) fed by backend data / websockets.
*/
const notificationsCount = ref(3);
const messagesCount = ref(1);

function toggleSearch() {
  isSearchOpen.value = !isSearchOpen.value;

  if (isSearchOpen.value) {
    nextTick(() => searchInput.value?.focus());
  } else {
    query.value = "";
  }
}

function closeSearch() {
  isSearchOpen.value = false;
  query.value = "";
}

function submitSearch() {
  if (!query.value) return;
  // We emit "search" using the template listeners in App.vue
  const q = query.value;
  closeSearch();
  // In <script setup>, we can emit from template via $emit,but for functions we usually capture emit = defineEmits(...).
  console.log("Search:", q);
}
</script>

<style scoped>
.topbar {
  position: sticky;
  top: 0;
  z-index: 50;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 0 14px;
  background: #0f172a;
  color: #e2e8f0;
  border-bottom: 1px solid rgba(148, 163, 184, 0.2);
}

.left,
.right {
  display: flex;
  align-items: center;
  min-width: 160px;
}

.brand {
  font-weight: 800;
  letter-spacing: 0.2px;
  color: inherit;
  text-decoration: none;
}

.center {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  flex: 1;
  min-width: 240px;
}

.icon-btn {
  position: relative;
  width: 40px;
  height: 40px;
  border: 0;
  background: transparent;
  color: inherit;
  border-radius: 12px;
  cursor: pointer;
  display: grid;
  place-items: center;
  text-decoration: none;
}

.icon-btn:hover {
  background: rgba(226, 232, 240, 0.08);
}

.icon {
  width: 22px;
  height: 22px;
}

.badge {
  position: absolute;
  top: 6px;
  right: 6px;
  min-width: 18px;
  height: 18px;
  padding: 0 5px;
  border-radius: 999px;
  background: #ef4444;
  color: white;
  font-size: 12px;
  line-height: 18px;
  font-weight: 800;
}

.search {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-left: 6px;
}

.search-input {
  width: 220px;
  height: 36px;
  border-radius: 12px;
  border: 1px solid rgba(148, 163, 184, 0.35);
  background: rgba(2, 6, 23, 0.6);
  color: #e2e8f0;
  padding: 0 10px;
  outline: none;
}

.search-input:focus {
  border-color: rgba(226, 232, 240, 0.7);
}

.search-go {
  height: 36px;
  padding: 0 12px;
  border-radius: 12px;
  border: 1px solid rgba(148, 163, 184, 0.35);
  background: rgba(226, 232, 240, 0.08);
  color: #e2e8f0;
  cursor: pointer;
}

.search-go:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.avatar {
  width: 36px;
  height: 36px;
  border-radius: 999px;
  border: 1px solid rgba(148, 163, 184, 0.35);
  background: rgba(226, 232, 240, 0.08);
  cursor: pointer;
  display: grid;
  place-items: center;
}

.avatar-dot {
  width: 12px;
  height: 12px;
  border-radius: 999px;
  background: rgba(226, 232, 240, 0.8);
}

@media (max-width: 520px) {
  .left,
  .right {
    min-width: 110px;
  }
  .search-input {
    width: 160px;
  }
}
</style>