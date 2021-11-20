<template>
  <div id="app">
    <SiteHeader @search-movies="search" />
    <SiteMain @find-cast="searchCastMovie" :Searched="allSearch" />

    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import axios from "axios";
import SiteMain from "./components/SiteMain.vue";
import SiteHeader from "./components/SiteHeader.vue";

export default {
  data() {
    return {
      searchSelected: "",
      movies: [],
      series: [],
    };
  },
  computed: {
    allSearch() {
      return [...this.movies, ...this.series];
    },
  },

  methods: {
    search(movie) {
      if (movie.length > 0) {
        this.searchSelected = movie;

        var config = {
          method: "get",
          url: `https://api.themoviedb.org/3/search/movie?api_key=33e1e99b35059079af3232f95a0930b3&query=${this.searchSelected}`,
        };

        var config_1 = {
          method: "get",
          url: `https://api.themoviedb.org/3/search/tv?api_key=33e1e99b35059079af3232f95a0930b3&language=en-US&query=${this.searchSelected}`,
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

        axios(config_1)
          .then((response) => {
            // console.log(response.data.results);
            this.series = response.data.results;
            // console.log(this.movies);
          })
          .catch(function (error) {
            console.log(error);
          });
      }
    },
    searchCastMovie(id) {
      let castMovie = [];
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${id}/credits?api_key=33e1e99b35059079af3232f95a0930b3`
        )
        .then((response) => {
          for (let i = 0; i < 5; i++) {
            castMovie.push(response.data.cast[i].name);
            // this.castMovie[index].push(response.data.cast[i].name);
          }
          console.log(castMovie);
          this.allSearch.forEach((movie) => {
            if (movie.id == id) {
              movie.cast = castMovie;
            }
          });
          console.log(this.allSearch);
        })
        .catch((e) => {
          console.log(e, "ERROR");
        });
    },
  },
  name: "App",
  components: {
    SiteHeader,
    SiteMain,
  },
};
</script>

<style lang="scss">
@import "./assets/scss/common.scss";
</style>
