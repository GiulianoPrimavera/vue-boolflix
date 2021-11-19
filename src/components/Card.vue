<template>
  <div class="single_card">
    <li>
      <p><strong>titolo:</strong> {{ title || name }}</p>
      <p>
        <strong>titolo originale:</strong> {{ original_title || original_name }}
      </p>
      <p>
        <strong>lingua originale:</strong>
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
        <strong>voto:</strong>
        <i
          class="fa-star"
          v-for="number in 5"
          :key="number"
          :class="number > getVoteFilms ? 'far' : 'fas'"
        ></i>
      </p>
      <div class="poster">
        <img
          :src="urlBasePoster + 'w342' + poster_path"
          :alt="title + ' poster'"
        />
      </div>
    </li>
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
  }
};
</script>

<style>
</style>