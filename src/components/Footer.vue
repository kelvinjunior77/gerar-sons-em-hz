<script setup>
import { ref } from 'vue';

var ano_atual = ref("");
ano_atual = new Date().getFullYear();

</script>

<template>
    <footer class="border-t border-gray-800 mt-16 py-8">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <div class="flex items-center space-x-2 mb-4 md:mb-0">
          <svg class="w-6 h-6 text-secondary" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"></path>
          </svg>
          <span class="text-lg font-semibold">SoundHz</span>
        </div>
        <div class="text-gray-400 text-sm">
          © 2025 - {{ano_atual}} SoundHz Generator.
        </div>
      </div>
    </div>
  </footer>
</template>