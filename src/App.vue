<template>
  <div id="app">

      <NavBar></NavBar>

      <!-- questa parte di ricerca verrà inserita nel file NavBar.vue -->
      <input
        type="text"
        placeholder="cerca un film"
        v-model="searchedString"
        @keyup.enter="search"
      />
      <button @click="search">cerca</button>

    <div class="main_container">
      <!-- movies -->
      <div class="movies_container">
        <h2>Movies</h2>
        <div class="films_container">
          <Card
            v-for="(film, i) in movies"
            :key="i"
            :title="film.title"
            :original_title="film.original_title"
            :original_language="film.original_language"
            :poster_path="film.poster_path"
            :vote_average="film.vote_average"
            :overview="film.overview"
          ></Card>
        </div>
      </div>

      <!-- tv series -->
      <div class="shows_container">
        <h2>TV Shows</h2>
        <div class="films_container">
          <Card
            v-for="(film, i) in tvShows"
            :key="i"
            :name="film.name"
            :original_name="film.original_name"
            :original_language="film.original_language"
            :poster_path="film.poster_path"
            :vote_average="film.vote_average"
            :overview="film.overview"
          ></Card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./components/Card.vue";
import NavBar from "./components/NavBar.vue";

export default {
  name: "App",
  components: {
    Card,
    NavBar
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
@import "styles/navbar.scss";
@import "styles/main_container.scss";
@import "styles/single_card.scss";
</style>