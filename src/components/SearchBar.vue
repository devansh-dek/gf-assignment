<!-- src/components/SearchBar.vue -->
<template>
    <div class="w-full max-w-xl mx-auto my-6">
      <div class="relative flex items-center">
        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </div>
        <input
          type="text"
          v-model="searchInput"
          @input="debounceSearch"
          placeholder="Search Wikipedia..."
          class="w-full pl-10 pr-4 py-3 rounded-full border border-gray-300 focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition-all duration-200 outline-none"
        />
        <button
          v-if="searchInput"
          @click="clearSearch"
          class="absolute right-3 text-gray-400 hover:text-gray-600"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watch } from 'vue';
  
  const searchInput = ref('');
  const emit = defineEmits(['search']);
  
  // Simple debounce function
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