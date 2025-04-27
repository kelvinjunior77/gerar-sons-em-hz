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

        
      <div class="text-center mb-4 md:mb-0">
        <p class="text-base text-gray-500 dark:text-gray-400">
          Desenvolvido por <span class="font-medium text-indigo-600 dark:text-indigo-400">Kelvin Junior</span>
        </p>
        
      </div>

        <div class="flex justify-cente space-x-6 mb-4 md:mb-0">
          <!-- GitHub -->
          <a 
            href="https://github.com/kelvinjunior77" 
            target="_blank"
            aria-label="GitHub"
            class="text-gray-400 hover:text-indigo-500 dark:hover:text-indigo-400 transition-colors duration-300 transform hover:scale-110"
          >
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
              <path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.919.678 1.852 0 1.336-.012 2.415-.012 2.743 0 .267.18.578.688.48A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" clip-rule="evenodd"/>
            </svg>
          </a>
          
          <!-- LinkedIn 
          <a 
            href="https://linkedin.com/in/seu-perfil" 
            target="_blank"
            aria-label="LinkedIn"
            class="text-gray-400 hover:text-indigo-500 dark:hover:text-indigo-400 transition-colors duration-300 transform hover:scale-110"
          >
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
              <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
            </svg>
          </a>-->
          
          <!-- Email -->
          <a 
            href="kelvinjunior592@gmail.com" 
            aria-label="Email"
            class="text-gray-400 hover:text-indigo-500 dark:hover:text-indigo-400 transition-colors duration-300 transform hover:scale-110"
          >
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 12.713l-11.985-9.713h23.97l-11.985 9.713zm0 2.574l-12-9.725v15.438h24v-15.438l-12 9.725z"/>
            </svg>
          </a>
          
          <!-- WhatsApp 
          <a 
            href="https://wa.me/5511999999999" 
            target="_blank"
            aria-label="WhatsApp"
            class="text-gray-400 hover:text-indigo-500 dark:hover:text-indigo-400 transition-colors duration-300 transform hover:scale-110"
          >
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
              <path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.245 2.248 3.481 5.236 3.48 8.414-.003 6.557-5.338 11.892-11.893 11.892-1.99-.001-3.951-.5-5.688-1.448l-6.305 1.654zm6.597-3.807c1.676.995 3.276 1.591 5.392 1.592 5.448 0 9.886-4.434 9.889-9.885.002-5.462-4.415-9.89-9.881-9.892-5.452 0-9.887 4.434-9.889 9.884-.001 2.225.651 3.891 1.746 5.634l-.999 3.648 3.742-.981zm11.387-5.464c-.074-.124-.272-.198-.57-.347-.297-.149-1.758-.868-2.031-.967-.272-.099-.47-.149-.669.149-.198.297-.768.967-.941 1.165-.173.198-.347.223-.644.074-.297-.149-1.255-.462-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.297-.347.446-.521.151-.172.2-.296.3-.495.099-.198.05-.372-.025-.521-.075-.148-.669-1.611-.916-2.206-.242-.579-.487-.501-.669-.51l-.57-.01c-.198 0-.52.074-.792.372s-1.04 1.016-1.04 2.479 1.065 2.876 1.213 3.074c.149.198 2.095 3.2 5.076 4.487.709.306 1.263.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.695.248-1.29.173-1.414z"/>
            </svg>
          </a>-->
        </div>

        
        <div class="ml-4 text-gray-400 text-sm ">
          © 2025 - {{ano_atual}} SoundHz
        </div>
      </div>
    </div>
  </footer>
</template>