<template>
  <div class="bg-blue-100 p-4 rounded-lg shadow-md max-w-lg mx-auto">
    <h3 class="text-xl font-semibold mb-2 text-blue-900">Historial de Búsquedas</h3>
    <ul>
      <li v-for="(item, index) in searchHistory" :key="index" class="capitalize text-blue-700 hover:text-blue-900 cursor-pointer">
        {{ item }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    searchHistory: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
</style>
