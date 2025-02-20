<template>
    <div 
      class="bg-white rounded-lg shadow-md p-4 mb-4 cursor-pointer transition-all duration-300 hover:shadow-lg"
      @click="toggleExpanded"
    >
      <div class="flex justify-between items-start">
        <div>
          <h3 class="text-lg font-semibold text-blue-700 mb-2" v-html="highlightMatch(result.title)"></h3>
          <p class="text-gray-600 text-sm" v-html="highlightMatch(result.snippet)"></p>
        </div>
        <div class="ml-4">
          <svg 
            xmlns="http://www.w3.org/2000/svg" 
            class="h-5 w-5 text-gray-400 transition-transform duration-300"
            :class="{ 'rotate-180': expanded }"
            viewBox="0 0 20 20" 
            fill="currentColor"
          >
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </div>
      </div>
      
      <transition
        enter-active-class="transition duration-200 ease-out"
        enter-from-class="transform scale-95 opacity-0"
        enter-to-class="transform scale-100 opacity-100"
        leave-active-class="transition duration-200 ease-in"
        leave-from-class="transform scale-100 opacity-100"
        leave-to-class="transform scale-95 opacity-0"
      >
        <div v-if="expanded" class="mt-4 pt-4 border-t border-gray-200">
          <div class="flex justify-between mb-2">
            <span class="text-sm text-gray-500">Wikipedia Page</span>
            <a 
              :href="`https://en.wikipedia.org/wiki/${encodeURIComponent(result.title)}`" 
              target="_blank"
              class="text-blue-600 hover:text-blue-800 text-sm flex items-center"
              @click.stop
            >
              Visit Page
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-1" viewBox="0 0 20 20" fill="currentColor">
                <path d="M11 3a1 1 0 100 2h2.586l-6.293 6.293a1 1 0 101.414 1.414L15 6.414V9a1 1 0 102 0V4a1 1 0 00-1-1h-5z" />
                <path d="M5 5a2 2 0 00-2 2v8a2 2 0 002 2h8a2 2 0 002-2v-3a1 1 0 10-2 0v3H5V7h3a1 1 0 000-2H5z" />
              </svg>
            </a>
          </div>
          <p class="text-sm text-gray-700">
            Word count: {{ countWords(result.snippet) }} words
          </p>
        </div>
      </transition>
    </div>
  </template>
  
  <script setup>
  import { ref, defineProps } from 'vue';
  
  const props = defineProps({
    result: {
      type: Object,
      required: true
    },
    searchQuery: {
      type: String,
      default: ''
    }
  });
  
  const expanded = ref(false);
  
  const toggleExpanded = () => {
    expanded.value = !expanded.value;
  };
  
  const countWords = (text) => {
    return text.split(/\s+/).filter(word => word.length > 0).length;
  };
  
  const highlightMatch = (text) => {
    if (!props.searchQuery) return text;
    
    const regex = new RegExp(`(${props.searchQuery})`, 'gi');
    return text.replace(regex, '<span class="bg-yellow-200">$1</span>');
  };
  </script>