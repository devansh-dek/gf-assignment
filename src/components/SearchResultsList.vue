<template>
  <div class="relative" ref="container">
    <div class="absolute inset-0 bg-gradient-to-b from-blue-50 to-transparent opacity-50 -z-10 rounded-xl"></div>
    
    <transition-group
      name="staggered-fade"
      tag="div"
      class="mt-4 space-y-4 relative"
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
    
    <div 
      v-if="loading"
      class="py-4 flex justify-center"
    >
      <div class="w-8 h-8 border-4 border-blue-500 border-t-transparent rounded-full animate-spin"></div>
    </div>
    
    <div 
      ref="sentinel"
      class="h-4 w-full"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
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
  }
});

const emit = defineEmits(['load-more']);
const sentinel = ref(null);
const container = ref(null);

// Intersection Observer for infinite scroll
let observer = null;

const observerCallback = (entries) => {
  const entry = entries[0];
  if (entry.isIntersecting && !props.loading) {
    emit('load-more');
  }
};

onMounted(() => {
  observer = new IntersectionObserver(observerCallback, {
    root: null,
    rootMargin: '100px',
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

// Animation methods
const beforeEnter = (el) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(30px)';
};

const enter = (el, done) => {
  const delay = el.dataset.index * 100;
  setTimeout(() => {
    el.style.opacity = 1;
    el.style.transform = 'translateY(0)';
    done();
  }, delay);
};

const leave = (el, done) => {
  el.style.opacity = 0;
  el.style.transform = 'translateY(30px)';
  setTimeout(done, 300);
};
</script>

<style scoped>
.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
.list-move {
  transition: transform 0.3s ease;
}
</style>