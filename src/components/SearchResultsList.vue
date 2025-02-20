<template>
    <transition-group
      name="list"
      tag="div"
      class="mt-4 space-y-4"
    >
      <SearchResultItem 
        v-for="result in results" 
        :key="result.pageid" 
        :result="result" 
        :searchQuery="searchQuery"
      />
    </transition-group>
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
  