<template>
  <div class="flex items-center justify-center h-screen bg-gradient-to-br from-gray-50 to-gray-100">
    <div class="relative">
      <div class="w-16 h-16 border-4 border-blue-500 border-t-transparent rounded-full animate-spin"></div>
      
      <div class="absolute inset-0 w-16 h-16 rounded-full animate-pulse bg-blue-300 opacity-30 -z-10"></div>
      
      <div class="absolute -top-2 -left-2 w-4 h-4 bg-blue-400 rounded-full animate-ping opacity-75 delay-300"></div>
      <div class="absolute top-2 -right-1 w-3 h-3 bg-blue-300 rounded-full animate-ping opacity-75 delay-700"></div>
      <div class="absolute -bottom-1 left-2 w-2 h-2 bg-blue-200 rounded-full animate-ping opacity-75 delay-1000"></div>
    </div>
  </div>
</template>