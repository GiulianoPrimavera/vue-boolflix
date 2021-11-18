<template>
  <div id="app">
    <div class="main_container">
      <input
        type="text"
        placeholder="cerca un film"
        v-model="searchedString"
        @keyup.enter="startSearching"
      />
      <button @click="startSearching">cerca</button>
      
      <!-- movies -->
      <ul>
        <h2>Movies</h2>
        <li v-for="(movie, i) in movies" :key="i">
          <p><strong>titolo:</strong> {{ movie.title }}</p>
          <p><strong>titolo originale:</strong> {{ movie.original_title }}</p>
          <!-- <p>lingua: <img :src="require('@/assets/' + movie.original_language + '.png' || '.jpg')" alt="">{{ flags[movie.original_language] }}</p> -->
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
          </p>
          <p><strong>voto:</strong> {{ getVote(i) }}</p>
          <div class="poster"><img :src="urlBasePoster + 'w342' + movie.poster_path" :alt="movie.title + ' poster'"></div>
        </li>
      </ul>

      <!-- tv series -->
      <ul>
        <h2>TV Shows</h2>
        <li v-for="(show, i) in tvShows" :key="i">
          <p><strong>titolo:</strong> {{ show.name }}</p>
          <p><strong>titolo originale:</strong> {{ show.original_name }}</p>
          <!-- <p>lingua: <img :src="require('@/assets/' + show.original_language + '.png' || '.jpg')" alt="">{{ flags[show.original_language] }}</p> -->
          <p>
            <strong>lingua originale:</strong>
            <img  class="flag_image"
              :src="
                !flags[show.original_language]
                  ? urlGeneric
                  : flags[show.original_language]
              "
              :alt="`${show.original_language} flag`"
            />
          </p>
          <p><strong>voto:</strong> {{ show.vote_average }}</p>
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
      urlBaseMovie: "https://api.themoviedb.org/3/search/movie?",
      urlBaseTvShow: "https://api.themoviedb.org/3/search/tv?",
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
    //eseguo la richiesta da axios all'api e popolo l'array "movies" ogni volta che premo enter o clicco su cerca
    startSearching() {
      //svuoto l'array all'inizio della funzione
      this.movies = [];
      //PER I FILM
      //eseguo la chiamata axios
      axios
        //come primo elemento della funzione get metto la prima parte necessaria per la ricerca di films (data dalla documentazione dell'api)
        .get(this.urlBaseMovie, {
          //come secondo elemento del get inserisco params (un oggetto nel quale vengono specificati dei parametri, axios "traduce" questi parametri in stringa di ricerca)
          params: {
            //this.apiKey è la chiave data dall'api
            api_key: this.apiKey,
            //this.searchedString è un v-model inserito nell'input di testo, query è il secondo parametro necessario per la ricerca dei film
            query: this.searchedString,
            language: "it",
          },
        })
        .then((resp) => {
          //prendo l'array di risposta dato dall'api e lo inseriso nel mio array movies
          this.movies.push(...resp.data.results);
        });
      console.log("array di film", this.movies);
    
    
      //svuoto l'array all'inizio della funzione
      this.tvShows = [];
      //PER LE SERIE
      //eseguo la chiamata axios
      axios
        //come primo elemento della funzione get metto la prima parte necessaria per la ricerca di serie (data dalla documentazione dell'api)
        .get(this.urlBaseTvShow, {
          //come secondo elemento del get inserisco params (un oggetto nel quale vengono specificati dei parametri, axios "traduce" questi parametri in stringa di ricerca)
          params: {
            //this.apiKey è la chiave data dall'api
            api_key: this.apiKey,
            //this.searchedString è un v-model inserito nell'input di testo, query è il secondo parametro necessario per la ricerca dei film
            query: this.searchedString,
            language: "it",
          },
        })
        .then((resp) => {
          //prendo l'array di risposta dato dall'api e lo inseriso nel mio array tvShows
          this.tvShows.push(...resp.data.results);
        });
      console.log("array di tv shows", this.tvShows);
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