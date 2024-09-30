<template>
    <div class="flex justify-center align-center p-4 gap-4">
        <div class="flex flex-col sm:w-auto">
          <input
            type="text"
            v-model="searchQuery"
            @input="fetchPokemonSuggestions"
            @keypress.enter="searchPokemon"
            placeholder="Buscar Pokémon..."
            class="w-screen max-w-lg px-4 py-2 border border-blue-300 bg-blue-100 text-blue-900 placeholder-blue-500 rounded-t-lg focus:outline-none focus:border-blue-400 transition duration-300"
          />
      
          <ul v-if="suggestions.length > 0" class="w-full max-w-lg border border-t-0 bg-white max-h-60 overflow-y-auto rounded-b-lg shadow-md">
            <li
              v-for="(suggestion, index) in suggestions"
              :key="index"
              @click="selectSuggestion(suggestion)"
              class="px-4 py-2 cursor-pointer hover:bg-blue-50 transition duration-200 ease-in-out"
            >
              {{ suggestion }}
            </li>
          </ul>
        </div>  
        <div>
          <button
            @click="searchPokemon"
            class=" px-6 py-2 bg-blue-400 text-white rounded-full shadow hover:bg-blue-500 transition duration-300 ease-in-out">Buscar</button>
            
        </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        searchQuery: '',
        suggestions: [],
        allPokemonNames: [],
      };
    },
    methods: {
      async fetchPokemonSuggestions() {
        if (!this.allPokemonNames.length) {
          const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=10000');
          this.allPokemonNames = response.data.results.map((pokemon) => pokemon.name);
        }
        if (this.searchQuery.length > 0) {
          this.suggestions = this.allPokemonNames.filter((name) =>
            name.toLowerCase().includes(this.searchQuery.toLowerCase())
          );
        } else {
          this.suggestions = [];
        }
      },
      selectSuggestion(suggestion) {
        this.searchQuery = suggestion;
        this.suggestions = [];
      },
      searchPokemon() {
        if (this.searchQuery.trim() !== '') {
          this.$emit('search', this.searchQuery);
          this.searchQuery = '';
          this.suggestions = [];
        }
      },
    },
  };
  </script>
  
