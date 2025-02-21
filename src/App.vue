<template>
  <div class="flex flex-col min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
    <div class="flex-grow container mx-auto px-4 py-8">
      <h1 class="text-3xl font-bold text-center text-gray-800 dark:text-gray-100 mb-6">
        <span class="text-blue-600 dark:text-blue-400">Wiki</span>Search
      </h1>

      <SearchBar @search="handleSearch" />
      
      <div class="mt-6 max-w-3xl  min-h-72 flex flex-col items-center justify-center  mx-auto ">
        <div v-if="error" class="bg-red-50 dark:bg-red-900/20 p-4 rounded-lg border border-red-200 dark:border-red-800">
          <div class="flex">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-red-500 dark:text-red-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
            </svg>
            <div class="ml-3">
              <h3 class="text-sm font-medium text-red-800 dark:text-red-200">Error</h3>
              <p class="mt-1 text-sm text-red-700 dark:text-red-300">{{ error }}</p>
              <button 
                @click="handleSearch(searchQuery)" 
                class="mt-2 text-sm text-red-600 dark:text-red-400 hover:text-red-800 dark:hover:text-red-300 font-medium"
              >
                Try again
              </button>
            </div>
          </div>
        </div>
        
        <div v-else-if="initialLoading" class="text-center py-16">
          <div class="relative w-20 h-20 mx-auto">
            <div class="w-20 h-20 border-4 border-blue-500 border-t-transparent rounded-full animate-spin"></div>
            <div class="absolute inset-0 flex items-center justify-center">
              <svg class="w-8 h-8 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
              </svg>
            </div>
          </div>
          <p class="mt-6 text-gray-500 dark:text-gray-400">Searching Wikipedia for "{{ searchQuery }}"...</p>
        </div>
        
        <div v-else-if="results.length > 0">
          <p class="text-sm text-gray-500 dark:text-gray-400 mb-4 flex justify-between items-center">
            <span>Found {{ totalResults }} results for "{{ searchQuery }}"</span>
            <button 
              @click="handleSearch(searchQuery)" 
              class="text-blue-600 dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300 flex items-center text-sm"
              :disabled="loading"
            >
              <svg xmlns="http://www.w3.org/2000/svg" 
                class="h-4 w-4 mr-1" 
                :class="{ 'animate-spin': loading }"
                fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
              </svg>
              {{ loading ? 'Refreshing...' : 'Refresh' }}
            </button>
          </p>
          
          <SearchResultsList 
            :results="results" 
            :searchQuery="searchQuery"
            :loading="loading"
            :totalResults="totalResults"
            @load-more="loadMoreResults"
          />
        </div>
        
        <div v-else-if="searchQuery && !loading && !initialLoading" class="text-center py-10 text-gray-500 dark:text-gray-400">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto text-gray-300 dark:text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <p class="mt-4 text-lg font-medium">No results found for "{{ searchQuery }}"</p>
          <p class="mt-2">Try different keywords or check your spelling</p>
        </div>
        
        <div v-else class="text-center py-10 h-full w-full flex justify-center flex-col items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 mx-auto text-gray-300 dark:text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <p class="mt-4 text-gray-500 dark:text-gray-400">Start typing to search Wikipedia</p>
        </div>
      </div>
    </div>
    
    <footer class="mt-auto py-4 border-t border-gray-200 dark:border-gray-800 text-center text-gray-600 dark:text-gray-400 text-sm">
      <p>Created with Vue.js and Tailwind CSS</p>
      <p class="mt-1">Data from Wikipedia API</p>
    </footer>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import SearchBar from './components/SearchBar.vue';
import SearchResultsList from './components/SearchResultsList.vue';

const searchQuery = ref('');
const results = ref([]);
const loading = ref(false);
const error = ref(null);
const currentOffset = ref(0);
const totalResults = ref(0);
const RESULTS_PER_PAGE = 10;

// Separate loading state for initial search vs pagination loading
const initialLoading = computed(() => {
  return loading.value && currentOffset.value === 0;
});

const handleSearch = async (query) => {
  if (query === searchQuery.value && loading.value) {
    return; // Prevent duplicate searches
  }
  
  searchQuery.value = query;
  error.value = null;
  currentOffset.value = 0;
  results.value = []; // Clear results immediately
  
  if (!query) {
    totalResults.value = 0;
    return;
  }
  
  // Set loading to true before fetching results
  loading.value = true;
  await fetchResults();
};

const fetchResults = async () => {
  try {
    // Add a small delay to show loading state (only for demo purposes)
    // In production, you'd rely on actual network time
    await new Promise(resolve => setTimeout(resolve, 300));
    
    const response = await fetch(
      `https://en.wikipedia.org/w/api.php?action=query&list=search&format=json&origin=*&srsearch=${encodeURIComponent(searchQuery.value)}&sroffset=${currentOffset.value}&srlimit=${RESULTS_PER_PAGE}`
    );
    
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    
    const data = await response.json();
    
    // Make sure we have valid data
    if (data && data.query && data.query.searchinfo) {
      totalResults.value = data.query.searchinfo.totalhits;
      
      const newResults = data.query.search.map(item => ({
        ...item,
        snippet: item.snippet.replace(/<\/?[^>]+(>|$)/g, ""),
      }));
      
      if (currentOffset.value === 0) {
        results.value = newResults;
      } else {
        // Only add non-duplicate results (using pageid as unique identifier)
        const existingIds = new Set(results.value.map(item => item.pageid));
        const uniqueNewResults = newResults.filter(item => !existingIds.has(item.pageid));
        results.value = [...results.value, ...uniqueNewResults];
      }
    } else {
      throw new Error('Invalid response format');
    }
  } catch (err) {
    console.error('Search error:', err);
    error.value = 'Failed to fetch search results. Please try again.';
  } finally {
    // Always set loading to false when done, whether success or error
    loading.value = false;
  }
};

const loadMoreResults = async () => {
  if (loading.value || results.value.length >= totalResults.value) return;
  
  // Set loading state before fetching more results
  loading.value = true;
  currentOffset.value += RESULTS_PER_PAGE;
  await fetchResults();
};
</script>