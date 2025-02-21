<template>
  <div 
    class="bg-white/90 dark:bg-gray-800/90 backdrop-blur-sm rounded-xl shadow-md hover:shadow-lg dark:shadow-gray-900/30 p-6 mb-5 cursor-pointer transition-all duration-300 border border-gray-100 dark:border-gray-700 hover:border-indigo-200 dark:hover:border-indigo-800"
    @click="toggleExpanded"
  >
    <div class="flex justify-between items-start gap-4">
      <div>
        <h3 class="text-xl font-medium text-indigo-700 dark:text-indigo-400 mb-3" v-html="highlightMatch(result.title)"></h3>
        <p class="text-gray-600 dark:text-gray-300 leading-relaxed" v-html="highlightMatch(result.snippet)"></p>
      </div>
      <div class="ml-2 mt-1">
        <div class="p-2 rounded-full bg-gray-50 dark:bg-gray-700 transition-transform duration-300 hover:bg-indigo-50 dark:hover:bg-indigo-900/30">
          <svg 
            xmlns="http://www.w3.org/2000/svg" 
            class="h-5 w-5 text-indigo-500 dark:text-indigo-400 transition-transform duration-300"
            :class="{ 'rotate-180': expanded }"
            viewBox="0 0 20 20" 
            fill="currentColor"
          >
            <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
          </svg>
        </div>
      </div>
    </div>
    
    <transition
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="transform scale-98 opacity-0"
      enter-to-class="transform scale-100 opacity-100"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="transform scale-100 opacity-100"
      leave-to-class="transform scale-98 opacity-0"
    >
      <div v-if="expanded" class="mt-5 pt-5 border-t border-gray-200 dark:border-gray-700">
        <div class="flex justify-between items-center mb-3">
          <span class="text-sm text-gray-500 dark:text-gray-400 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
            </svg>
            Wikipedia Page
          </span>
          <a 
            :href="`https://en.wikipedia.org/wiki/${encodeURIComponent(result.title)}`" 
            target="_blank"
            class="text-indigo-600 dark:text-indigo-400 hover:text-indigo-800 dark:hover:text-indigo-300 font-medium text-sm flex items-center px-4 py-2 rounded-lg bg-indigo-50 dark:bg-indigo-900/30 hover:bg-indigo-100 dark:hover:bg-indigo-800/40 transition-colors duration-200"
            @click.stop
          >
            Visit Page
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 ml-1.5" viewBox="0 0 20 20" fill="currentColor">
              <path d="M11 3a1 1 0 100 2h2.586l-6.293 6.293a1 1 0 101.414 1.414L15 6.414V9a1 1 0 102 0V4a1 1 0 00-1-1h-5z" />
              <path d="M5 5a2 2 0 00-2 2v8a2 2 0 002 2h8a2 2 0 002-2v-3a1 1 0 10-2 0v3H5V7h3a1 1 0 000-2H5z" />
            </svg>
          </a>
        </div>
        <div class="bg-gray-50 dark:bg-gray-700/50 rounded-lg p-3 mt-3">
          <p class="text-sm text-gray-700 dark:text-gray-300 flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-2 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 8h10M7 12h4m1 8l-4-4H5a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v8a2 2 0 01-2 2h-3l-4 4z" />
            </svg>
            Word count: <span class="font-medium ml-1">{{ countWords(result.snippet) }} words</span>
          </p>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue';

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
  return text.replace(regex, '<span class="bg-indigo-100 dark:bg-indigo-900/30 text-indigo-800 dark:text-indigo-300 px-1 rounded">$1</span>');
};
</script>