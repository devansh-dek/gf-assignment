<template>
  <div class="min-h-screen bg-gray-50">
    <div class="container mx-auto px-4 py-8">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">
        <span class="text-blue-600">Wiki</span>Search
      </h1>

      <SearchBar @search="handleSearch" />
      
      <div class="mt-6 max-w-3xl mx-auto">
        <Loader v-if="loading" />
        
        <div v-else-if="results.length > 0">
          <p class="text-sm text-gray-500 mb-4">
            Found {{ results.length }} results for "{{ searchQuery }}"
          </p>
          <SearchResultsList :results="results" :searchQuery="searchQuery" />
        </div>
        
        <div v-else-if="searchQuery" class="text-center py-10 text-gray-500">
          No results found for "{{ searchQuery }}"
        </div>
        
        <div v-else class="text-center py-10">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <p class="mt-4 text-gray-500">Start typing to search Wikipedia</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import SearchBar from './components/SearchBar.vue';
import SearchResultsList from './components/SearchResultsList.vue';
import Loader from './components/Loader.vue';

const searchQuery = ref('');
const results = ref([]);
const loading = ref(false);

const handleSearch = async (query) => {
  searchQuery.value = query;
  
  if (!query) {
    results.value = [];
    return;
  }
  
  loading.value = true;
  
  try {
    // Wikipedia API call
    const response = await fetch(
      `https://en.wikipedia.org/w/api.php?action=query&list=search&format=json&origin=*&srsearch=${encodeURIComponent(query)}`
    );
    const data = await response.json();
    
    // Process results
    results.value = data.query.search.map(item => ({
      ...item,
      snippet: item.snippet.replace(/<\/?[^>]+(>|$)/g, ""), // Remove HTML tags
    }));
    
    // Add slight delay to show loading effect for better UX
    setTimeout(() => {
      loading.value = false;
    }, 300);
  } catch (error) {
    console.error('Search error:', error);
    results.value = [];
    loading.value = false;
  }
};
</script>
