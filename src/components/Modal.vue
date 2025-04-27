<template>
    <transition name="modal">
      <div v-if="isOpen" class="fixed inset-0 z-50 flex items-center justify-center p-4">
        <!-- Fundo escuro -->
        <div class="fixed inset-0 bg-black/50" @click="close"></div>
        
        <!-- Conteúdo -->
        <div class="relative z-10 w-full max-w-md bg-white dark:bg-gray-800 rounded-lg shadow-xl">
          <!-- Header -->
          <div class="flex items-center justify-between p-4 border-b dark:border-gray-700">
            <h3 class="text-lg font-semibold">{{ title }}</h3>
            <button @click="close" class="text-gray-500 hover:text-gray-700 dark:hover:text-gray-300">
              &times;
            </button>
          </div>
          
          <!-- Corpo -->
          <div class="p-4">
            <slot></slot>
          </div>
        </div>
      </div>
    </transition>
  </template>
  
  <script setup>
  defineProps({
    isOpen: Boolean,
    title: String
  })
  
  const emit = defineEmits(['close'])
  
  const close = () => emit('close')
  </script>