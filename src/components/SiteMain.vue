<template>
  <main>
    <SearchMovies @search-movies="search" />
    <div class="container">
      <ul class="row">
        <li class="col-3" v-for="movie in allSearch" :key="movie.id">
          <div class="movie">
            <div class="img_wrapper" v-if="movie.poster_path !== null">
              <img
                :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path"
                alt=""
              />
            </div>
            <div v-else>
              <img
                width="342"
                height="513"
                src="https://developers.google.com/maps/documentation/maps-static/images/error-image-generic.png?hl=it"
                alt=""
              />
            </div>

            <div class="title">
              <span>Titolo: </span>
              <span>{{ movie.title || movie.name }}</span>
            </div>
            <div class="original-title">
              <span>Titolo Originale: </span>
              <span>{{ movie.original_title || movie.original_name }}</span>
            </div>
            <div class="language">
              <span>Lingua: </span>

              <span v-if="movie.original_language == 'en'">
                <flag iso="gb" />
              </span>
              <span v-else-if="movie.original_language == 'zh'">
                <flag iso="cn" />
              </span>
              <span v-else-if="movie.original_language == 'ja'">
                <flag iso="jp" />
              </span>
              <span v-else-if="movie.original_language == 'da'">
                <flag iso="dm" />
              </span>
              <span v-else>
                <flag :iso="movie.original_language" />
              </span>
              <span> {{ movie.original_language }}</span>
            </div>
            <div class="vote">
              <div v-if="parseInt((movie.vote_average / 2).toFixed(0)) !== 0">
                <span>Voto: </span>
                <span
                  v-for="n in parseInt((movie.vote_average / 2).toFixed(0))"
                  :key="n.id"
                  >⭐</span
                >
              </div>
              <div v-else>
                <span>Voto: </span>
                <span>0</span>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </main>
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
      series: [],
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
  },

  computed: {
    allSearch() {
      return [...this.movies, ...this.series];
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