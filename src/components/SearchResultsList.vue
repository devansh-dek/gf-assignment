<template>
  <div class="relative">
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
    
    <transition
      enter-active-class="transition duration-700 ease-out"
      enter-from-class="opacity-0 translate-y-4"
      enter-to-class="opacity-100 translate-y-0"
      leave-active-class="transition duration-300 ease-in"
      leave-from-class="opacity-100"
      leave-to-class="opacity-0"
    >
      <div v-if="results.length === 0 && searchQuery" class="py-12 flex flex-col items-center justify-center text-gray-500">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 text-gray-300 mb-4 animate-pulse" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
        </svg>
        <p class="text-lg">No results found for "{{ searchQuery }}"</p>
        <p class="text-sm mt-2">Try a different search term</p>
      </div>
    </transition>
  </div>
</template>
  
  <script setup>
  import { defineProps } from 'vue';
  import SearchResultItem from './SearchResultItem.vue';
  
  defineProps({
    results: {
      type: Array,
      required: true
    },
    searchQuery: {
      type: String,
      default: ''
    }
  });
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
  