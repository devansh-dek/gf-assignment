<template>
  <div class="min-h-screen bg-gray-50">
    <div class="container mx-auto px-4 py-8">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">
        <span class="text-blue-600">Wiki</span>Search
      </h1>

      <SearchBar @search="handleSearch" />
      
      <div class="mt-6 max-w-3xl mx-auto">
        <div v-if="error" class="bg-red-50 p-4 rounded-lg border border-red-200">
          <div class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
            </svg>
            <div class="ml-3">
              <h3 class="text-sm font-medium text-red-800">Error</h3>
              <p class="mt-1 text-sm text-red-700">{{ error }}</p>
              <button 
                @click="handleSearch(searchQuery)" 
                class="mt-2 text-sm text-red-600 hover:text-red-800 font-medium"
              >
                Try again
              </button>
            </div>
          </div>
        </div>
        
        <div v-else-if="results.length > 0">
          <p class="text-sm text-gray-500 mb-4 flex justify-between items-center">
            <span>Found {{ totalResults }} results for "{{ searchQuery }}"</span>
            <button 
              @click="handleSearch(searchQuery)" 
              class="text-blue-600 hover:text-blue-800 flex items-center text-sm"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
              Refresh
            </button>
          </p>
          <SearchResultsList 
            :results="results" 
            :searchQuery="searchQuery"
            :loading="loading"
            @load-more="loadMoreResults"
          />
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
    
    <footer class="mt-10 py-4 border-t border-gray-200 text-center text-gray-600 text-sm">
      <p>Created with Vue.js and Tailwind CSS</p>
      <p class="mt-1">Data from Wikipedia API</p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import SearchBar from './components/SearchBar.vue';
import SearchResultsList from './components/SearchResultsList.vue';

const searchQuery = ref('');
const results = ref([]);
const loading = ref(false);
const error = ref(null);
const currentOffset = ref(0);
const totalResults = ref(0);
const RESULTS_PER_PAGE = 10;

const handleSearch = async (query) => {
  searchQuery.value = query;
  error.value = null;
  currentOffset.value = 0;
  
  if (!query) {
    results.value = [];
    return;
  }
  
  await fetchResults();
};

const fetchResults = async () => {
  loading.value = true;
  
  try {
    const response = await fetch(
      `https://en.wikipedia.org/w/api.php?action=query&list=search&format=json&origin=*&srsearch=${encodeURIComponent(searchQuery.value)}&sroffset=${currentOffset.value}&srlimit=${RESULTS_PER_PAGE}`
    );
    
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    
    const data = await response.json();
    totalResults.value = data.query.searchinfo.totalhits;
    
    const newResults = data.query.search.map(item => ({
      ...item,
      snippet: item.snippet.replace(/<\/?[^>]+(>|$)/g, ""),
    }));
    
    if (currentOffset.value === 0) {
      results.value = newResults;
    } else {
      results.value = [...results.value, ...newResults];
    }
    
    loading.value = false;
  } catch (err) {
    console.error('Search error:', err);
    error.value = 'Failed to fetch search results. Please try again.';
    loading.value = false;
  }
};

const loadMoreResults = async () => {
  if (loading.value || results.value.length >= totalResults.value) return;
  
  currentOffset.value += RESULTS_PER_PAGE;
  await fetchResults();
};
</script>