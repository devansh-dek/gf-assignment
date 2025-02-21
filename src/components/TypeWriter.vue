<template>
    <p class="mt-4 text-gray-500 dark:text-gray-400 font-mono flex items-center">
      {{ displayText }}
      <span 
        class="inline-block w-0.5 h-5 ml-1 bg-gray-400 dark:bg-gray-500"
        :class="{ 'opacity-0': !cursorVisible }"
      ></span>
    </p>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  
  const props = defineProps({
    text: {
      type: String,
      default: 'Start typing to search Wikipedia'
    },
    typingSpeed: {
      type: Number,
      default: 100
    },
    cursorBlinkSpeed: {
      type: Number,
      default: 530
    }
  });
  
  const displayText = ref('');
  const cursorVisible = ref(true);
  let cursorInterval = null;
  
  const typeText = async () => {
    for (let i = 0; i <= props.text.length; i++) {
      displayText.value = props.text.substring(0, i);
      await new Promise(resolve => setTimeout(resolve, props.typingSpeed));
    }
  };
  

  const startCursorBlink = () => {
    cursorInterval = setInterval(() => {
      cursorVisible.value = !cursorVisible.value;
    }, props.cursorBlinkSpeed);
  };
  
  onMounted(() => {
    typeText();
    startCursorBlink();
  });
  
  onUnmounted(() => {
    if (cursorInterval) {
      clearInterval(cursorInterval);
    }
  });
  </script>