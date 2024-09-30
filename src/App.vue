<template>
  <div class="min-h-screen bg-blue-50 ">

    <div class="flex aling-center justify-center w-full bg-blue-600">
      <img src="./assets/img/pokemon-logo-pokemon-icon-transparent-free-png.webp" alt="TITLE" width="150px">
    </div>

    <SearchBar @search="searchPokemon" />

    <div class="flex justify-center">
      <button
        @click="toggleSearchHistory"
        class="px-6 py-2 bg-blue-400 text-white rounded-full shadow-md hover:bg-blue-500 transition duration-300 ease-in-out"
      >
        {{ showSearchHistory ? 'Ocultar Historial' : 'Mostrar Historial' }}
      </button>
    </div>


    <SearchHistory v-if="showSearchHistory" :searchHistory="searchHistory" />

    <div class="flex justify-center mb-6">
      <button
        v-if="showResetButton"
        @click="resetToDefault"
        class="px-6 py-2 bg-red-500 text-white rounded-full shadow-md hover:bg-red-700 transition duration-300 ease-in-out"
      >
        Regresar al inicio
      </button>
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 px-10">
      <PokemonCard v-for="pokemon in pokemons" :key="pokemon.id" :pokemon="pokemon" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';
import SearchBar from './components/SearchBar.vue';
import SearchHistory from './components/SearchHistory.vue';

export default {
  components: {
    PokemonCard,
    SearchBar,
    SearchHistory,
  },
  data() {
    return {
      pokemons: [],
      searchHistory: [],
      defaultPokemons: ['ditto', 'pikachu', 'bulbasaur', 'charmander'],
      showResetButton: false,
      showSearchHistory: false, 
    };
  },
  methods: {
    async searchPokemon(query) {
      if (query.trim() !== '') {
        try {
          const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${query.toLowerCase()}`);
          this.pokemons = [response.data];

          this.showResetButton = true;
          this.updateSearchHistory(query);
        } catch (error) {
          alert('Pokémon no encontrado');
        }
      }
    },
    updateSearchHistory(query) {
      this.searchHistory.unshift(query);
      if (this.searchHistory.length > 5) {
        this.searchHistory.pop();
      }
      localStorage.setItem('searchHistory', JSON.stringify(this.searchHistory));
    },
    loadSearchHistory() {
      const storedHistory = localStorage.getItem('searchHistory');
      if (storedHistory) {
        this.searchHistory = JSON.parse(storedHistory);
      }
    },
    toggleSearchHistory() {
      this.showSearchHistory = !this.showSearchHistory; 
    },
    async resetToDefault() {
      this.pokemons = await Promise.all(
        this.defaultPokemons.map(async (name) => {
          const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${name}`);
          return response.data;
        })
      );
      this.showResetButton = false;
    },
  },
  async mounted() {
    this.loadSearchHistory();
    this.pokemons = await Promise.all(
      this.defaultPokemons.map(async (name) => {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${name}`);
        return response.data;
      })
    );
  },
};
</script>

<style scoped>
</style>
