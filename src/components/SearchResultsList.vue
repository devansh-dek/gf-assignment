<template>
  <div class="relative w-full max-w-5xl mx-auto px-4" ref="container">
    <div class="absolute inset-0 bg-gradient-to-b from-indigo-50/50 via-white/20 to-transparent dark:from-indigo-900/20 dark:via-gray-900/10 -z-10 rounded-2xl"></div>
    
    <transition-group
      name="staggered-fade"
      tag="div"
      class="mt-6 space-y-5 relative"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave"
    >
      <SearchResultItem
        v-for="(result, index) in results"
        :key="result.pageid"
        :result="result"
        :searchQuery="searchQuery"
        :data-index="index"
        class="transition-all duration-500 ease-out"
      />
    </transition-group>
    
    <div v-if="loading" class="py-12 flex justify-center items-center">
      <div class="flex flex-col items-center">
        <div class="relative">
          <div class="w-12 h-12 border-4 border-indigo-500 border-t-transparent rounded-full animate-spin"></div>
          <div class="absolute inset-0 w-12 h-12 rounded-full animate-pulse bg-indigo-200 dark:bg-indigo-700 opacity-30 -z-10"></div>
        </div>
        <p class="mt-4 text-gray-600 dark:text-gray-400 font-medium">Loading more results...</p>
      </div>
    </div>
    
    <div v-if="hasMoreResults && !loading" class="mt-8  text-center">
      <button 
        @click="manualLoadMore"
        class="px-8 py-3.5 bg-indigo-600 hover:bg-indigo-700 focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-50 text-white rounded-xl transition duration-200 flex items-center mx-auto shadow-md hover:shadow-lg"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
        </svg>
        Load More Results
      </button>
    </div>
    
    <div v-if="!hasMoreResults && results.length > 0 && !loading" class="py-8 text-center">
      <div class="inline-flex items-center px-6 py-3 bg-gray-50 dark:bg-gray-800 rounded-full text-gray-600 dark:text-gray-300">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
        </svg>
        You've reached the end of the results
      </div>
    </div>
    
    <!-- <div ref="sentinel" class="h-20 w-full"></div> -->
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed, watch } from 'vue';
import SearchResultItem from './SearchResultItem.vue';

const props = defineProps({
  results: {
    type: Array,
    required: true
  },
  searchQuery: {
    type: String,
    default: ''
  },
  loading: {
    type: Boolean,
    default: false
  },
  totalResults: {
    type: Number,
    default: 0
  }
});

const emit = defineEmits(['load-more']);
const sentinel = ref(null);
const container = ref(null);
const autoLoadEnabled = ref(false);

const hasMoreResults = computed(() => {
  return props.results.length > 0 && props.results.length < props.totalResults;
});

const manualLoadMore = () => {
  if (!props.loading && hasMoreResults.value) {
    window.scrollBy({
      top: 100,
      behavior: 'smooth'
    });
    
    emit('load-more');
  }
};

// Auto-load function
const autoLoadMore = () => {
  if (autoLoadEnabled.value && !props.loading && hasMoreResults.value) {
    emit('load-more');
  }
};

// Intersection Observer for infinite scroll
let observer = null;

const observerCallback = (entries) => {
  const entry = entries[0];
  if (entry.isIntersecting) {
    autoLoadMore();
  }
};

onMounted(() => {
  observer = new IntersectionObserver(observerCallback, {
    root: null,
    rootMargin: '300px',
    threshold: 0.1
  });
    
  if (sentinel.value) {
    observer.observe(sentinel.value);
  }
});

onUnmounted(() => {
  if (observer) {
    observer.disconnect();
  }
});

// Reset auto-load when search query changes
watch(() => props.searchQuery, () => {
  autoLoadEnabled.value = false;
});

// Improved animation methods
const beforeEnter = (el) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(25px)';
};

const enter = (el, done) => {
  const delay = Math.min(el.dataset.index * 80, 400);
  setTimeout(() => {
    el.style.transition = 'all 300ms cubic-bezier(0.4, 0, 0.2, 1)';
    el.style.opacity = 1;
    el.style.transform = 'translateY(0)';
    setTimeout(done, 300);
  }, delay);
};

const leave = (el, done) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(25px)';
  setTimeout(done, 250);
};
</script>

<style scoped>
.list-enter-active, .list-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}
.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(25px);
}
.list-move {
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}
</style>