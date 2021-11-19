<template>
  <div id="app">
    <div class="main_container">
      <input
        type="text"
        placeholder="cerca un film"
        v-model="searchedString"
        @keyup.enter="search"
      />
      <button @click="search">cerca</button>

      <!-- movies -->
      <ul>
        <h2>Movies</h2>
        <Card
          v-for="(film, i) in movies"
          :key="i"
          :title="film.title"
          :original_title="film.original_title"
          :original_language="film.original_language"
          :poster_path="film.poster_path"
          :vote_average="film.vote_average"
        ></Card>
      </ul>

      <!-- tv series -->
      <ul>
        <h2>TV Shows</h2>
        <Card
          v-for="(film, i) in tvShows"
          :key="i"
          :name="film.name"
          :original_name="film.original_name"
          :original_language="film.original_language"
          :poster_path="film.poster_path"
          :vote_average="film.vote_average"
        ></Card>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./components/Card.vue";

export default {
  name: "App",
  components: {
    Card,
  },
  data() {
    return {
      apiKey: "cd46b1fc41160a5e0f5ace06fece242a",
      movies: [],
      tvShows: [],
      searchedString: "",
    };
  },
  methods: {
    genericSearch(baseUrl, stringaDaCercare, genericArray) {
      axios
        .get(baseUrl, {
          params: {
            api_key: this.apiKey,
            query: stringaDaCercare,
          },
        })
        .then((resp) => {
          this[genericArray] = resp.data.results;
        });
    },
    search() {
      this.genericSearch(
        "https://api.themoviedb.org/3/search/movie?",
        this.searchedString,
        "movies"
      );
      this.genericSearch(
        "https://api.themoviedb.org/3/search/tv?",
        this.searchedString,
        "tvShows"
      );
    },
  },
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