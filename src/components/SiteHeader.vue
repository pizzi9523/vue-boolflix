<template>
  <header>
    <SearchMovies @search-movies="search" />
    <div class="container">
      <ul class="row">
        <li class="col-3" v-for="movie in movies" :key="movie.id">
          <div class="movie">
            <div class="title">Titolo: {{ movie.title }}</div>
            <div class="original-title">
              Titolo Originale: {{ movie.original_title }}
            </div>
            <div class="language">
              Lingua: <flag :iso="movie.original_language" />
            </div>
            <div class="vote">Voto: {{ movie.vote_average }}</div>
          </div>
        </li>
      </ul>
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
      if (movie.length > 0) {
        this.searchSelected = movie;

        var config = {
          method: "get",
          url: `https://api.themoviedb.org/3/search/movie?api_key=33e1e99b35059079af3232f95a0930b3&query=${this.searchSelected}`,
        };

        axios(config)
          .then((response) => {
            // console.log(response.data.results);
            this.movies = response.data.results;
            // console.log(this.movies);
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    },
  },
};
</script>

<style scoped lang="scss">
ul {
  list-style: none;
  .movie {
    padding: 5px;
  }
}
</style>