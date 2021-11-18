<template>
  <header>
    <SearchMovies @search-movies="search" />
    <div></div>
  </header>
</template>

<script>
// import axios from "axios";
import SearchMovies from "./SearchMovies.vue";
export default {
  data() {
    return {
      searchSelected: "",
    };
  },
  components: {
    SearchMovies,
  },
  methods: {
    search(movie) {
      this.searchSelected = movie;
    },
  },

  computed: {},
};
</script>

<style>
</style>