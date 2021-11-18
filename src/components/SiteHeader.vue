<template>
  <header>
    <SearchMovies @search-movies="search" />
    <div class="container">
      <div class="row">
        <div class="col">
          <div class="movie" v-for="movie in movies" :key="movie.id">
            <div class="title">{{ movie.title }}</div>
            <div class="original-title">{{ movie.original_title }}</div>
            <div class="language">{{ movie.original_language }}</div>
            <div class="vote">{{ movie.vote_average }}</div>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import axios from "axios";
import SearchMovies from "./SearchMovies.vue";
export default {
  mounted() {
    setTimeout(this.callApi, 1 * 1000);
  },

  data() {
    return {
      searchSelected: "",
      movies: [],
    };
  },
  components: {
    SearchMovies,
  },

  methods: {
    search(movie) {
      this.searchSelected = movie;
    },

    callApi() {
      var config = {
        method: "get",
        url: "https://api.themoviedb.org/3/search/movie?api_key=33e1e99b35059079af3232f95a0930b3&query=ritorno+al+futuro",
      };

      axios(config)
        .then((response) => {
          // console.log(response.data.results);
          this.movies = response.data.results;
          console.log(this.movies);
        })
        .catch(function (error) {
          console.log(error);
        });
    },
  },
};
</script>

<style>
.movie {
}
</style>