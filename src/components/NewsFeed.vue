<template>
  <div class="max-w-2xl mx-auto pb-20">
    <div ref="feedContainer">
      <NewsCard
        v-for="item in displayedNews"
        :key="item.id"
        :news="item"
      />
    </div>

    <div
      v-if="loading"
      class="flex justify-center items-center py-8"
    >
      <div class="animate-spin rounded-full h-10 w-10 border-b-2 border-purple-600"></div>
    </div>

    <div
      v-if="!loading && hasMore"
      ref="loadMoreTrigger"
      class="h-10"
    ></div>

    <div
      v-if="!hasMore && displayedNews.length > 0"
      class="text-center py-8 text-gray-500 text-sm"
    >
      You've reached the end of the feed
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import NewsCard from './NewsCard.vue';
import type { NewsItem } from '@/types/news';

const displayedNews = ref<NewsItem[]>([]);
const allNews = ref<NewsItem[]>([]);
const loading = ref(false);
const hasMore = ref(true);
const loadMoreTrigger = ref<HTMLElement | null>(null);
const feedContainer = ref<HTMLElement | null>(null);

const ITEMS_PER_LOAD = 3;
let currentIndex = 0;
let observer: IntersectionObserver | null = null;

const loadNews = async () => {
  try {
    const response = await fetch('/sampleData.json');
    const data = await response.json();
    allNews.value = data.news;
    loadMoreItems();
  } catch (error) {
    console.error('Failed to load news:', error);
  }
};

const loadMoreItems = () => {
  if (loading.value || !hasMore.value) return;

  loading.value = true;

  setTimeout(() => {
    const nextItems = allNews.value.slice(
      currentIndex,
      currentIndex + ITEMS_PER_LOAD
    );

    displayedNews.value.push(...nextItems);
    currentIndex += ITEMS_PER_LOAD;

    if (currentIndex >= allNews.value.length) {
      hasMore.value = false;
    }

    loading.value = false;
  }, 500);
};

const setupIntersectionObserver = () => {
  observer = new IntersectionObserver(
    (entries) => {
      const entry = entries[0];
      if (entry.isIntersecting && hasMore.value && !loading.value) {
        loadMoreItems();
      }
    },
    {
      root: null,
      rootMargin: '100px',
      threshold: 0.1,
    }
  );

  if (loadMoreTrigger.value) {
    observer.observe(loadMoreTrigger.value);
  }
};

onMounted(() => {
  loadNews();
  setupIntersectionObserver();
});

onUnmounted(() => {
  if (observer) {
    observer.disconnect();
  }
});
</script>
