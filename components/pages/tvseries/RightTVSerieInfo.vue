<template>
  <div>
    <h1 v-if="getDirectors.length" class="grey--text title">Director</h1>
    <span v-for="(director, index) in getDirectors" :key="index">
      {{ director.name }}
      <span v-if="getDirectors.length - 1 > index">, </span>
    </span>
    <h1 class="grey--text title">Genre</h1>
    <span v-for="(genre, index) in getGenres" :key="'genre'+index">
      {{ genre.name }}
      <span v-if="getGenres.length - 1 > index">, </span>
    </span>
    <h1 class="grey--text title">First Air Date</h1>
    <span>
      {{ getFirstAirDate }}
    </span>
    <div v-if="getEpisodes.length">
      <h1 class="grey--text title">Number Of Episodes</h1>
      <span> {{ getEpisodes }} episodes </span>
    </div>
    <h1 class="grey--text title">Spoken Language</h1>
    <span v-for="(language, index) in getLanguages" :key="'language'+index">
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
      return directors;
    },
    getGenres() {
      let genres = [];
      for (const item of this.data.genres) {
        genres.push(item);
      }
      return genres;
    },
    getFirstAirDate() {
      return this.data.first_air_date;
    },
    getEpisodes() {
      return this.data.episode_run_time;
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
