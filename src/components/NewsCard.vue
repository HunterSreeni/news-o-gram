<template>
  <article class="bg-white mb-4 last:mb-0">
    <div class="px-4 py-3 flex items-center space-x-3">
      <div class="w-8 h-8 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 flex items-center justify-center text-white font-bold text-sm">
        {{ news.source.charAt(0) }}
      </div>
      <div class="flex-1">
        <p class="font-semibold text-sm text-gray-900">{{ news.source }}</p>
        <p class="text-xs text-gray-500">{{ formatTime(news.timestamp) }}</p>
      </div>
    </div>

    <div class="w-full aspect-square bg-gray-200">
      <img
        :src="news.imageUrl"
        :alt="news.headline"
        class="w-full h-full object-cover"
        loading="lazy"
      />
    </div>

    <div class="px-4 py-3">
      <div class="flex items-center space-x-4 mb-3">
        <button
          @click="toggleShare"
          class="p-1 hover:text-gray-500 transition-colors"
          aria-label="Share"
        >
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" />
          </svg>
        </button>
      </div>

      <h2 class="font-bold text-lg text-gray-900 mb-2">
        {{ news.headline }}
      </h2>

      <p class="text-sm text-gray-700 mb-3">
        {{ news.description }}
      </p>

      <div class="flex flex-wrap gap-2 mb-2">
        <span
          v-for="tag in news.tags"
          :key="tag"
          class="px-2 py-1 bg-gray-100 text-xs text-gray-700 rounded-full"
        >
          #{{ tag.replace(/\s+/g, '') }}
        </span>
      </div>

      <a
        :href="news.articleUrl"
        target="_blank"
        rel="noopener noreferrer"
        class="text-sm text-purple-600 hover:text-purple-700 font-medium inline-flex items-center"
      >
        Read full article
        <svg class="w-4 h-4 ml-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
        </svg>
      </a>
    </div>

    <div
      v-if="showShare"
      class="px-4 pb-4"
    >
      <div class="bg-gray-50 rounded-lg p-4 space-y-3">
        <div class="flex items-center justify-between">
          <p class="text-sm font-semibold text-gray-700">Share this article</p>
          <button
            @click="toggleShare"
            class="text-gray-500 hover:text-gray-700"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>

        <div class="flex items-center space-x-2">
          <input
            :value="news.articleUrl"
            readonly
            class="flex-1 px-3 py-2 text-sm bg-white border border-gray-300 rounded-lg"
          />
          <button
            @click="copyUrl"
            class="px-4 py-2 bg-purple-600 text-white text-sm font-medium rounded-lg hover:bg-purple-700 transition-colors"
          >
            {{ copied ? 'Copied!' : 'Copy' }}
          </button>
        </div>

        <div class="grid grid-cols-5 gap-2">
          <a
            :href="getShareUrl('facebook')"
            target="_blank"
            rel="noopener noreferrer"
            class="flex flex-col items-center justify-center p-3 hover:bg-gray-200 rounded-lg transition-colors"
            aria-label="Share on Facebook"
          >
            <svg class="w-6 h-6 text-blue-600" fill="currentColor" viewBox="0 0 24 24">
              <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
            </svg>
            <span class="text-xs mt-1">Facebook</span>
          </a>

          <a
            :href="getShareUrl('twitter')"
            target="_blank"
            rel="noopener noreferrer"
            class="flex flex-col items-center justify-center p-3 hover:bg-gray-200 rounded-lg transition-colors"
            aria-label="Share on Twitter"
          >
            <svg class="w-6 h-6 text-sky-500" fill="currentColor" viewBox="0 0 24 24">
              <path d="M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z"/>
            </svg>
            <span class="text-xs mt-1">Twitter</span>
          </a>

          <a
            :href="getShareUrl('linkedin')"
            target="_blank"
            rel="noopener noreferrer"
            class="flex flex-col items-center justify-center p-3 hover:bg-gray-200 rounded-lg transition-colors"
            aria-label="Share on LinkedIn"
          >
            <svg class="w-6 h-6 text-blue-700" fill="currentColor" viewBox="0 0 24 24">
              <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
            </svg>
            <span class="text-xs mt-1">LinkedIn</span>
          </a>

          <a
            :href="getShareUrl('reddit')"
            target="_blank"
            rel="noopener noreferrer"
            class="flex flex-col items-center justify-center p-3 hover:bg-gray-200 rounded-lg transition-colors"
            aria-label="Share on Reddit"
          >
            <svg class="w-6 h-6 text-orange-600" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 0A12 12 0 0 0 0 12a12 12 0 0 0 12 12 12 12 0 0 0 12-12A12 12 0 0 0 12 0zm5.01 4.744c.688 0 1.25.561 1.25 1.249a1.25 1.25 0 0 1-2.498.056l-2.597-.547-.8 3.747c1.824.07 3.48.632 4.674 1.488.308-.309.73-.491 1.207-.491.968 0 1.754.786 1.754 1.754 0 .716-.435 1.333-1.01 1.614a3.111 3.111 0 0 1 .042.52c0 2.694-3.13 4.87-7.004 4.87-3.874 0-7.004-2.176-7.004-4.87 0-.183.015-.366.043-.534A1.748 1.748 0 0 1 4.028 12c0-.968.786-1.754 1.754-1.754.463 0 .898.196 1.207.49 1.207-.883 2.878-1.43 4.744-1.487l.885-4.182a.342.342 0 0 1 .14-.197.35.35 0 0 1 .238-.042l2.906.617a1.214 1.214 0 0 1 1.108-.701zM9.25 12C8.561 12 8 12.562 8 13.25c0 .687.561 1.248 1.25 1.248.687 0 1.248-.561 1.248-1.249 0-.688-.561-1.249-1.249-1.249zm5.5 0c-.687 0-1.248.561-1.248 1.25 0 .687.561 1.248 1.249 1.248.688 0 1.249-.561 1.249-1.249 0-.687-.562-1.249-1.25-1.249zm-5.466 3.99a.327.327 0 0 0-.231.094.33.33 0 0 0 0 .463c.842.842 2.484.913 2.961.913.477 0 2.105-.056 2.961-.913a.361.361 0 0 0 .029-.463.33.33 0 0 0-.464 0c-.547.533-1.684.73-2.512.73-.828 0-1.979-.196-2.512-.73a.326.326 0 0 0-.232-.095z"/>
            </svg>
            <span class="text-xs mt-1">Reddit</span>
          </a>

          <a
            :href="getShareUrl('whatsapp')"
            target="_blank"
            rel="noopener noreferrer"
            class="flex flex-col items-center justify-center p-3 hover:bg-gray-200 rounded-lg transition-colors"
            aria-label="Share on WhatsApp"
          >
            <svg class="w-6 h-6 text-green-600" fill="currentColor" viewBox="0 0 24 24">
              <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"/>
            </svg>
            <span class="text-xs mt-1">WhatsApp</span>
          </a>
        </div>
      </div>
    </div>
  </article>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { NewsItem } from '@/types/news';

interface Props {
  news: NewsItem;
}

const props = defineProps<Props>();

const showShare = ref(false);
const copied = ref(false);

const toggleShare = () => {
  showShare.value = !showShare.value;
  if (!showShare.value) {
    copied.value = false;
  }
};

const copyUrl = async () => {
  try {
    await navigator.clipboard.writeText(window.location.href);
    copied.value = true;
    setTimeout(() => {
      copied.value = false;
    }, 2000);
  } catch (err) {
    console.error('Failed to copy:', err);
  }
};

const formatTime = (timestamp: string) => {
  const date = new Date(timestamp);
  const now = new Date();
  const diffInMs = now.getTime() - date.getTime();
  const diffInHours = Math.floor(diffInMs / (1000 * 60 * 60));

  if (diffInHours < 1) {
    const diffInMinutes = Math.floor(diffInMs / (1000 * 60));
    return `${diffInMinutes}m ago`;
  } else if (diffInHours < 24) {
    return `${diffInHours}h ago`;
  } else {
    const diffInDays = Math.floor(diffInHours / 24);
    return `${diffInDays}d ago`;
  }
};

const getShareUrl = (platform: string) => {
  const url = encodeURIComponent(window.location.href);
  const text = encodeURIComponent(`${props.news.headline} - News-O-Gram`);

  const shareUrls: Record<string, string> = {
    facebook: `https://www.facebook.com/sharer/sharer.php?u=${url}`,
    twitter: `https://twitter.com/intent/tweet?url=${url}&text=${text}`,
    linkedin: `https://www.linkedin.com/sharing/share-offsite/?url=${url}`,
    reddit: `https://www.reddit.com/submit?url=${url}&title=${text}`,
    whatsapp: `https://wa.me/?text=${text}%20${url}`,
  };

  return shareUrls[platform] || '#';
};
</script>
