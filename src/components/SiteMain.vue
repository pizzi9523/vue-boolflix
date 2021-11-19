<template>
  <main>
    <div class="container">
      <ul class="row" v-if="this.Searched.length > 0">
        <li
          class="col-3"
          v-for="(movie, index) in this.Searched"
          :key="movie.id"
          :index="index"
        >
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
                  <div
                    v-if="parseInt((movie.vote_average / 2).toFixed(0)) !== 0"
                  >
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
                <div v-if="movie.overview.length < 600" class="overview">
                  {{ movie.overview }}
                </div>
                <div v-else class="overview">
                  {{ movie.overview.substr(0, 600) + "..." }}
                </div>
                <div class="cast">
                  <span>Cast: </span>
                  <span v-for="actor in castMovie" :key="actor">
                    {{ actor }},
                  </span>
                  <button @click="findCast(movie.id, index)">Show Cast</button>
                </div>
              </div>
            </div>
          </div>
        </li>
      </ul>
      <div class="nothing" v-else>Nothing To Show 😢</div>
    </div>
  </main>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      castMovie: [],
      pointer: 0,
    };
  },
  props: {
    Searched: Array,
  },

  methods: {
    findCast(id, index) {
      console.log(id);
      this.castMovie = [];
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${id}/credits?api_key=33e1e99b35059079af3232f95a0930b3`
        )
        .then((response) => {
          for (let i = 0; i < 5; i++) {
            this.castMovie.push(response.data.cast[i].name);
          }
          console.log(this.castMovie);
          this.pointer = index;
          console.log(index);
          console.log(this.pointer);

          //console.log(castMovie);
        });
    },
  },
};
</script>

<style  lang="scss">
main {
  background-color: rgb(92, 91, 91);
  padding: 50px;
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
      }
      .info_movie {
        padding: 20px;
        width: 100%;
        height: 100%;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: black;
        color: white;
        background-size: cover;
        display: flex;
        flex-direction: column;
        justify-content: center;
        font-size: 18px;
        visibility: hidden;
      }
    }
  }
}

.nothing {
  text-align: center;
  font-size: 30px;
}
</style>