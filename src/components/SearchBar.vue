<template>
  <div class="w-full max-w-3xl mx-auto my-8 px-4">
    <div class="relative flex items-center">
      <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-indigo-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
        </svg>
      </div>
      <input
        type="text"
        v-model="searchInput"
        @input="debounceSearch"
        placeholder="Search Wikipedia..."
        class="w-full pl-12 pr-12 py-4 rounded-xl border-0 bg-white/90 backdrop-blur-sm shadow-lg focus:ring-2 focus:ring-indigo-500 focus:shadow-indigo-100/50 focus:shadow-xl transition-all duration-300 outline-none dark:bg-gray-800/90 dark:text-white dark:placeholder-gray-400"
      />
      <button
        v-if="searchInput"
        @click="clearSearch"
        class="absolute right-4 text-gray-400 hover:text-indigo-600 transition-colors duration-200"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  </div>
</template>
  
<script setup>
import { ref } from 'vue';
  
const searchInput = ref('');
const emit = defineEmits(['search']);
  
// Improved debounce function
let debounceTimeout;
const debounceSearch = () => {
  clearTimeout(debounceTimeout);
  debounceTimeout = setTimeout(() => {
    emit('search', searchInput.value);
  }, 300);
};
  
const clearSearch = () => {
  searchInput.value = '';
  emit('search', '');
};
</script>