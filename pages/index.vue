<template>
  <v-container>
    <section-title title="Popular Movies" link="/movies" />
    <v-row v-if="movies.length">
      <v-col cols="12" sm="4" v-for="movie in movies" :key="movie.id">
        <movie-card :movie="movie" />
      </v-col>
    </v-row>
    <section-title title="Upcoming Movies" link="/movies" />
    <v-row v-if="movies.length">
      <v-col cols="12" sm="4" v-for="item in upcoming" :key="item.id">
        <movie-card :movie="item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import MovieCard from "~/components/MovieCard.vue";
import SectionTitle from "~/components/SectionTitle.vue";
export default {
  components: { MovieCard, SectionTitle },
  async asyncData({ $axios }) {
    try {
      const res = await $axios.$get("/movie/popular");
      const res2 = await $axios.$get("/movie/upcoming");
      return {
        movies: res.results.slice(0, 6),
        upcoming: res2.results.slice(0, 6),
      };
    } catch (e) {
      console.log(e.message);
      return { movies: [], upcoming: [] };
    }
  },
};
</script>

<style></style>
