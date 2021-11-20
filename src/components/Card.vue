<template>
  <div class="single_card">
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
      <p>{{ overview }}</p>
    </div>
    <div class="poster">
      <img
        :src="urlBasePoster + 'w342' + poster_path"
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
    };
  },
  computed: {
    getVoteFilms() {
      return Math.ceil(this.vote_average / 2);
    },
  },
};
</script>

<style>
</style>