<template>
  <div class="single_card"  :class="poster_path === null ? 'poster_null' : ''">
    <div class="description">
      <h2>{{ title || name }}</h2>
      <h3>{{ original_title || original_name }}</h3>
      <p>
        <img
          :class="flags[original_language] ? 'flag_image' : 'lang_not_found'"
          :src="
            !flags[original_language] ? urlGeneric : flags[original_language]
          "
          :alt="`${original_language} flag`"
        />
        <span v-if="!flags[original_language]">{{ original_language }}</span>
      </p>
      <p>
        <i
          class="fa-star"
          v-for="number in 5"
          :key="number"
          :class="number > getVoteFilms ? 'far' : 'fas'"
        ></i>
      </p>
      <p><strong>Trama</strong><br>{{ overview }}</p>
    </div>
    <div class="poster">
        <!-- :src="urlBasePoster + 'w342' + poster_path" -->
      <img
        :src="getPoster"
        :alt="title + ' poster'"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    title: String,
    original_title: String,
    original_language: String,
    poster_path: String,
    name: String,
    original_name: String,
    vote_average: Number,
    overview: String,
  },
  data() {
    return {
      flags: {
        en: require("@/assets/en.png"),
        es: require("@/assets/es.png"),
        fr: require("@/assets/fr.jpg"),
        it: require("@/assets/it.png"),
      },
      urlGeneric: require("@/assets/notfound.jpg"),
      urlBasePoster: "https://image.tmdb.org/t/p/",
      posterWidth: "w342"
    };
  },
  computed: {
    getVoteFilms() {
      return Math.ceil(this.vote_average / 2);
    },
    getPoster(){
      return this.urlBasePoster + this.posterWidth + this.poster_path
    }
  },
  methods: {
  }
};
</script>

<style>
</style>