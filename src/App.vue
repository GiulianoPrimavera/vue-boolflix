<template>
  <div id="app">
    <div class="main_container">
      <input
        type="text"
        placeholder="cerca un film"
        v-model="searchedString"
        @keyup.enter="usaLaSearch"
      />
      <button @click="usaLaSearch">cerca</button>
      
      <!-- movies -->
      <ul>
        <h2>Movies</h2>
        <li v-for="(movie, i) in movies" :key="i">
          <p><strong>titolo:</strong> {{ movie.title }}</p>
          <p><strong>titolo originale:</strong> {{ movie.original_title }}</p>
          <p>
            <strong>lingua originale:</strong>
            <img :class="flags[movie.original_language] ? 'flag_image' : 'lang_not_found'"
              :src="
                !flags[movie.original_language]
                  ? urlGeneric
                  : flags[movie.original_language]
              "
              :alt="`${movie.original_language} flag`"
            /> 
            <span v-if="!flags[movie.original_language]">{{movie.original_language}}</span>
          </p>
          <p><strong>voto:</strong> 
          <!-- se il numero della stella che sta stampando è maggiore del numero dei voti ottenuti allora stampa stelle vuote, altrimenti stelle piene -->
            <i class="fa-star" v-for="number in 5" :key="number"
              :class="number > getVote(i) ? 'far' : 'fas'"
            ></i>
            {{ getVote(i) }}
          </p>
          <div class="poster"><img :src="urlBasePoster + 'w342' + movie.poster_path" :alt="movie.title + ' poster'"></div>
        </li>
      </ul>

      <!-- tv series -->
      <ul>
        <h2>TV Shows</h2>
        <li v-for="(show, i) in tvShows" :key="i">
          <p><strong>titolo:</strong> {{ show.name }}</p>
          <p><strong>titolo originale:</strong> {{ show.original_name }}</p>
          <p>
            <strong>lingua originale:</strong>
            <img  :class="flags[show.original_language] ? 'flag_image' : 'lang_not_found'"
              :src="
                !flags[show.original_language]
                  ? urlGeneric
                  : flags[show.original_language]
              "
              :alt="`${show.original_language} flag`"
            />
            <span v-if="!flags[show.original_language]">{{show.original_language}}</span>
          </p>
          <p><strong>voto:</strong> 
          <!-- se il numero della stella che sta stampando è maggiore del numero dei voti ottenuti allora stampa stelle vuote, altrimenti stelle piene -->
            <i class="fa-star" v-for="number in 5" :key="number"
              :class="number > getVoteShow(i) ? 'far' : 'fas'"
            ></i>
            {{ getVoteShow(i) }}
          </p>
          <div class="poster"><img :src="urlBasePoster + 'w342' + show.poster_path" :alt="show.title + ' poster'"></div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      apiKey: "cd46b1fc41160a5e0f5ace06fece242a",
      movies: [],
      tvShows: [],
      flags: {
        en: require("@/assets/en.png") /* '@/assets/en.png' */,
        es: require("@/assets/es.png") /* '@/assets/es.png' */,
        fr: require("@/assets/fr.jpg") /* '@/assets/fr.jpg' */,
        it: require("@/assets/it.png") /* '@/assets/it.png' */,
      },
      urlGeneric: require("@/assets/notfound.jpg"),
      searchedString: "",
      urlBasePoster: "https://image.tmdb.org/t/p/"
    };
  },
  computed: {
    //quando faccio le card questa funzione posso inserirla nelle computed
    /* getVote(i) {
      return Math.ceil(this.movies[i].vote_average / 2);
    }, */
  },
  methods: {
    getVote(i) {
      return Math.ceil(this.movies[i].vote_average / 2);
    },
    getVoteShow(i) {
      return Math.ceil(this.tvShows[i].vote_average / 2);
    },
    genericSearch(baseUrl, stringaDaCercare, genericArray){

      axios.get(baseUrl, {
        params: {
          api_key: this.apiKey,
          query: stringaDaCercare
        }
      }).then((resp) => {
        this[genericArray] = resp.data.results
      })
    },
    usaLaSearch(){
      this.genericSearch("https://api.themoviedb.org/3/search/movie?", this.searchedString, "movies")
      this.genericSearch("https://api.themoviedb.org/3/search/tv?", this.searchedString, "tvShows")
    }
  }
};
</script>

<style lang="scss">
@import "styles/app.scss";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>