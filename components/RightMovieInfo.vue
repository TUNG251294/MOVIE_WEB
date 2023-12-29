<template>
  <div>
    <h1 class="grey--text title">Director</h1>
    <span v-for="(director, index) in getDirectors" :key="index">
      {{ director.name }}
      <span v-if="getDirectors.length - 1 > index">, </span>
    </span>
    <h1 class="grey--text title">Genre</h1>
    <span v-for="(genre, index) in getGenres" :key="index">
      {{ genre.name }}
      <span v-if="getGenres.length - 1 > index">, </span>
    </span>
    <h1 class="grey--text title">Release Date</h1>
    <span>
      {{ getReleaseDate }}
    </span>
    <h1 class="grey--text title">Runtime</h1>
    <span> {{ getRuntime }} mins </span>
    <h1 class="grey--text title">Spoken Language</h1>
    <span v-for="(language, index) in getLanguages" :key="index">
      {{ language.name }}
      <span v-if="getLanguages.length - 1 > index">, </span>
    </span>
  </div>
</template>

<script>
export default {
  props: {
    data: { type: Object },
  },
  computed: {
    getDirectors() {
      let directors = [];
      for (const item of this.data.credits.crew) {
        if (item.job === "Director") {
          directors.push(item);
        }
      }
      console.log(directors);
      return directors;
    },
    getGenres() {
      let genres = [];
      for (const item of this.data.genres) {
        genres.push(item);
      }
      return genres;
    },
    getReleaseDate() {
      return this.data.release_date;
    },
    getRuntime() {
      return this.data.runtime;
    },
    getLanguages() {
      let languages = [];
      for (const item of this.data.spoken_languages) {
        languages.push(item);
      }
      return languages;
    },
  },
};
</script>

<style></style>
