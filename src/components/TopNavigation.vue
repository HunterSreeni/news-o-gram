<template>
  <nav class="sticky top-0 z-50 bg-white border-b border-gray-300">
    <div class="max-w-2xl mx-auto px-4 py-3 flex items-center justify-between">
      <div class="flex items-center space-x-2">
        <svg class="w-8 h-8 text-purple-600" fill="currentColor" viewBox="0 0 24 24">
          <path d="M12 2L2 7v10c0 5.55 3.84 10.74 9 12 5.16-1.26 9-6.45 9-12V7l-10-5zm0 18c-3.31 0-6-2.69-6-6s2.69-6 6-6 6 2.69 6 6-2.69 6-6 6zm0-10c-2.21 0-4 1.79-4 4s1.79 4 4 4 4-1.79 4-4-1.79-4-4-4z"/>
        </svg>
        <h1 class="text-xl font-bold text-gray-900">News-O-Gram</h1>
      </div>

      <button
        @click="toggleSearch"
        class="p-2 rounded-full hover:bg-gray-100 transition-colors"
        aria-label="Search"
      >
        <svg class="w-6 h-6 text-gray-700" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
        </svg>
      </button>
    </div>

    <div
      v-if="showSearch"
      class="max-w-2xl mx-auto px-4 pb-3"
    >
      <input
        type="text"
        placeholder="Search news... (Coming soon)"
        class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent"
        disabled
      />
    </div>
  </nav>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const showSearch = ref(false);

const toggleSearch = () => {
  showSearch.value = !showSearch.value;
};
</script>
