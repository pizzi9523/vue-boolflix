<template>
  <main>
    <FilterFilmGenres @filter-genre="searchByFilter" :genres="allGenres" />

    <div class="container">
      <ul class="row" v-if="this.filterGenres.length > 0">
        <li class="col-3" v-for="movie in this.filterGenres" :key="movie.id">
          <div class="movie">
            <div class="img_wrapper">
              <img
                height="513"
                width="342"
                v-if="movie.poster_path !== null"
                :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path"
                alt=""
              />
              <img
                v-else
                width="342"
                height="513"
                src="https://developers.google.com/maps/documentation/maps-static/images/error-image-generic.png?hl=it"
                alt=""
              />
              <div class="info_movie">
                <div class="title">
                  <span>Titolo: </span>
                  <span>{{ movie.title || movie.name }}</span>
                </div>
                <!-- /.title  -->
                <div class="original-title">
                  <span>Titolo Originale: </span>
                  <span>{{ movie.original_title || movie.original_name }}</span>
                </div>
                <!-- /.original-title  -->
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
                </div>
                <!-- /.language -->
                <div class="vote">
                  <div
                    v-if="parseInt((movie.vote_average / 2).toFixed(0)) !== 0"
                  >
                    <span>Voto: </span>
                    <span
                      v-for="n in parseInt((movie.vote_average / 2).toFixed(0))"
                      :key="n.id"
                      ><i class="fa fa-star" aria-hidden="true"></i
                    ></span>
                    <span
                      v-for="n in 5 -
                      parseInt((movie.vote_average / 2).toFixed(0))"
                      :key="n.id"
                      ><i class="far fa-star"></i
                    ></span>
                  </div>
                  <div v-else>
                    <span>Voto: </span>
                    <span>0</span>
                  </div>
                </div>
                <!-- /.vote  -->
                <div v-if="movie.overview.length < 1000" class="overview">
                  {{ movie.overview }}
                </div>
                <div v-else class="overview">
                  {{ movie.overview.substr(0, 1000) + "..." }}
                </div>
                <!-- /.overview  -->
                <div class="genres">
                  Genere:
                  <span v-for="genre in allGenres" :key="genre.id">
                    <span v-if="movie.genre_ids.includes(genre.id)">
                      {{ genre.name }}
                    </span>
                  </span>
                </div>
                <!-- /.genres  -->
                <div class="cast">
                  <span>Cast: </span>

                  <span v-for="actor in movie.cast" :key="actor">
                    {{ actor }}
                  </span>
                  <button
                    class="show_cast"
                    v-show="!movie.cast"
                    @click="$emit('find-cast', movie.id)"
                  >
                    Show Cast
                  </button>
                </div>
                <!-- /.cast  -->
              </div>
              <!-- /.info_movie  -->
            </div>
            <!-- /.img_wrapper  -->
          </div>
          <!-- /.movie  -->
        </li>
        <!-- /.col-3  -->
      </ul>
      <!-- /.row  -->
      <div class="nothing" v-else>Nothing To Show 😢</div>
    </div>
    <!-- /.container  -->
  </main>
</template>

<script>
import axios from "axios";
import FilterFilmGenres from "./FilterFilmGenres.vue";
export default {
  data() {
    return {
      // castMovie: [],
      pointer: 0,
      allGenres: [],
      selectedGenres: "",
    };
  },

  props: {
    Searched: Array,
  },

  mounted() {
    axios
      .get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=33e1e99b35059079af3232f95a0930b3"
      )
      .then((response) => {
        this.allGenres = response.data.genres;
        //console.log(this.allGenres);
      });
  },

  computed: {
    filterGenres() {
      if (this.selectedGenres === "") {
        return this.Searched;
      } else {
        const filteredList = this.Searched.filter((movie) => {
          return movie.genre_ids.includes(this.selectedGenres);
        });
        return filteredList;
      }
    },
  },

  methods: {
    searchByFilter(id) {
      //console.log(id);
      this.selectedGenres = id;
    },
    /* findCast(id, index) {
      this.pointer = index;
      console.log(index);
      console.log(this.pointer);
      console.log(id);
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${id}/credits?api_key=33e1e99b35059079af3232f95a0930b3`
        )
        .then((response) => {
          this.castMovie[index] = [];
          for (let i = 0; i < 5; i++) {
            this.castMovie[index].push(response.data.cast[i].name);
            // this.castMovie[index].push(response.data.cast[i].name);
          }
          console.log(this.castMovie);

          //console.log(castMovie);
        })
        .catch((e) => {
          console.log(e, "ERROR");
        });
    },*/
  },

  components: {
    FilterFilmGenres,
  },
};
</script>

<style  lang="scss">
main {
  background-color: rgb(59, 58, 58);
  padding: 0 50px;
  min-height: calc(100vh - 76px);

  nav {
    padding: 25px 0;
  }
}
ul {
  list-style: none;
  .movie {
    padding: 5px;
    .img_wrapper {
      width: fit-content;
      position: relative;

      &:hover {
        .info_movie {
          visibility: visible;
        }
        img {
          filter: brightness(0.3);
        }
      }
      .info_movie {
        padding: 20px;
        width: 100%;
        height: 100%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        color: white;
        background-color: black;
        background-size: cover;
        display: flex;
        flex-direction: column;
        justify-content: center;
        font-size: 18px;
        visibility: hidden;
        div {
          padding: 4px 0;
        }
        .overview {
          overflow-y: auto;
          &::-webkit-scrollbar {
            width: 10px;
          }
          &::-webkit-scrollbar-track {
            box-shadow: inset 0 0 5px white;
            border-radius: 10px;
          }
          &::-webkit-scrollbar-thumb {
            background: red;
            border-radius: 10px;
          }

          ::-webkit-scrollbar-thumb:hover {
            background: #b30000;
          }
        }
        .fa-star {
          color: gold;
        }
        button.show_cast {
          padding: 0rem 0.5rem;
          background-color: transparent;
          color: white;
          border: none;
          font-weight: 600;
          &:hover {
            cursor: pointer;
            text-decoration: underline;
          }
        }
      }
    }
  }
}

.nothing {
  text-align: center;
  font-size: 30px;
  color: white;
}
</style>