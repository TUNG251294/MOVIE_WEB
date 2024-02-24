<template>
  <v-row>
    <v-col cols="6" sm="3" v-for="movie in movies" :key="movie.id">
      <MovieCard :movie="movie" :genres="genres"/>
    </v-col>
  </v-row>
</template>

<script>
import MovieCard from '~/components/pages/movies/MovieCard.vue'
export default {
  components: { MovieCard },
  props: {
    movies: { type: Array }
  },
  data(){
    return{
      genres: []
    }
  },
  async fetch() {
    await this.$axios
      .$get('/genre/movie/list')
      .then((response) => {
        this.genres = response.genres
      })
      .catch((e)=>{
        console.log(e)
      })
  }
}
</script>