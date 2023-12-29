<template>
  <v-card :to="`/movies/${movie.id}`">
    <v-img
      cover
      :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
    ></v-img>
    <v-card-title>{{ movie.title }}</v-card-title>
    <v-card-text>
      <v-row no-gutters>
        <v-rating
          half-increments
          length="5"
          size="16"
          color="yellow"
          dense
          readonly
          :value="movie.vote_average / 2"
        ></v-rating>
        <span class="grey--text text-lighten-1 text-caption mx-2 mt-1">{{
          movie.vote_average
        }}</span>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style></style>
