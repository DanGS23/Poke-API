<template>
  <div class="bg-blue-200 text-blue-900 p-4 rounded-lg shadow-md hover:shadow-lg transition duration-300">
    <img :src="pokemon.sprites.front_default" alt="Pokemon Image" class="w-48 h-48 mx-auto" />
    <h2 class="text-center text-xl font-semibold mt-2 capitalize">{{ pokemon.name }}</h2>
    <p class="text-center mt-1">
      ID: {{ pokemon.id }}
    </p>
    <p class="text-center capitalize">
      Tipo: {{ pokemon.types[0].type.name }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },
};
</script>

