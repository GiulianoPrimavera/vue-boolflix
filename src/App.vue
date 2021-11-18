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
      <ul>
        <li v-for="(movie, i) in movies" :key="i">
          <p>titolo: {{ movie.original_title }}</p>
          <p>titolo originale: {{ movie.title }}</p>
          <!-- <p>lingua: <img :src="require('@/assets/' + movie.original_language + '.png' || '.jpg')" alt="">{{ flags[movie.original_language] }}</p> -->
          <p>lingua: <img :src="flags[movie.original_language]" :alt="`${movie.original_language} flag`"></p>
          <p>voto: {{ movie.vote_average }}</p>
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
      flags: {
        en: require("@/assets/en.png")/* '@/assets/en.png' */,
        es: require("@/assets/es.png")/* '@/assets/es.png' */,
        fr: require("@/assets/fr.jpg")/* '@/assets/fr.jpg' */,
        it: require("@/assets/it.png")/* '@/assets/it.png' */
      },
      searchedString: "",
    };
  },
  computed: {
   /*  singleFlag(){
      return this.flags
    } */
  },
  methods: {
    //eseguo la richiesta da axios all'api e popolo l'array "movies" ogni volta che premo enter o clicco su cerca
    startSearching() {
      //svuoto l'array all'inizio della funzione
      this.movies = [];

      //eseguo la chiamata axios
      axios
        //come primo elemento della funzione get metto la prima parte necessaria per la ricerca di films (data dalla documentazione dell'api)
        .get("https://api.themoviedb.org/3/search/movie?", {
          //come secondo elemento del get inserisco params (un oggetto nel quale vengono specificati dei parametri, axios "traduce" questi parametri in stringa di ricerca)
          params: {
            //this.apiKey è la chiave data dall'api
            api_key: this.apiKey,
            //this.searchedString è un v-model inserito nell'input di testo, query è il secondo parametro necessario per la ricerca dei film
            query: this.searchedString,
          },
        })
        .then((resp) => {
          //prendo l'array di risposta dato dall'api e lo inseriso nel mio array movies
          this.movies.push(...resp.data.results);
        });
      console.log("array di film", this.movies);
    },
  },
  mounted() {
    /* axios
    .get("https://api.themoviedb.org/3/search/movie?", {
      params: {
        api_key: this.apiKey,
        query: this.searchedString
    }})
    .then(resp => {
      this.movies.push(...resp.data.results) 
    });
    console.log("array di prova", this.movies); */
  },
};
</script>

<style lang="scss">
@import 'styles/app.scss';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
