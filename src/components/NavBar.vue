<template>
  <div class="navbar">
    <div class="logo_title">
      <h1>BOOLFLIX</h1>
    </div>
    <div class="search_bar_container">
        <!-- qui ci sarà la barra di ricerca -->
        <input
        type="text"
        placeholder="cerca un film"
        v-model="searchedString"
        @keyup.enter="search"
      />
      <button @click="search">cerca</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  props: {
    genericSearch: Function
  },
  data(){
    return {
      searchedString: "",
    }
  },
  methods: {
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
  }
};
</script>

<style>
</style>