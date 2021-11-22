<template>
  <nav class="filter">
    <select
      v-model="genreSelected"
      name="Generi"
      id=""
      @change="$emit('filter-genre', genreSelected)"
    >
      <option value="">All</option>
      <option v-for="genre in genres" :key="genre.id" :value="genre.id">
        {{ genre.name }}
      </option>
    </select>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      genreSelected: "",
    };
  },
  props: {
    genres: Array,
  },
};
</script>

<style>
</style>