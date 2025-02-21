<template>
  <div class="flex items-center justify-center h-screen bg-gradient-to-br from-indigo-50 to-white dark:from-gray-900 dark:to-gray-800">
    <div class="relative">
      <div class="w-20 h-20 border-4 border-indigo-500 border-t-transparent rounded-full animate-spin"></div>
      
      <div class="absolute inset-0 w-20 h-20 rounded-full animate-pulse bg-indigo-300 dark:bg-indigo-700 opacity-30 -z-10"></div>
      
      <div class="absolute -top-3 -left-3 w-5 h-5 bg-indigo-400 rounded-full animate-ping opacity-75 delay-300"></div>
      <div class="absolute top-3 -right-2 w-4 h-4 bg-indigo-300 rounded-full animate-ping opacity-75 delay-700"></div>
      <div class="absolute -bottom-2 left-3 w-3 h-3 bg-indigo-200 dark:bg-indigo-600 rounded-full animate-ping opacity-75 delay-1000"></div>
      
      <div class="absolute -bottom-12 left-1/2 transform -translate-x-1/2 whitespace-nowrap">
        <p class="text-indigo-700 dark:text-indigo-300 font-medium animate-pulse">Loading Wikipedia...</p>
      </div>
    </div>
  </div>
</template>