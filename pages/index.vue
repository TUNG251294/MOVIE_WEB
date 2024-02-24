<template>
  <v-container>
    <SectionTitle title="Popular Movies" link="/movies"/>
    <v-row v-if="!movies.length">
      <v-col cols="12" sm="3" v-for="i in 8" :key="i">
        <v-skeleton-loader type="image, list-item-two-line" />
      </v-col>
    </v-row>
    <SectionMovies v-else :movies="movies" />
    <SectionTitle title="Upcoming Movies" link="/movies" />
    <v-row v-if="!upcoming.length">
      <v-col cols="12" sm="3" v-for="i in 8" :key="i">
        <v-skeleton-loader type="image, list-item-two-line" />
      </v-col>
    </v-row>
    <SectionMovies v-else :movies="upcoming" />
  </v-container>
</template>

<script>
import SectionTitle from "~/components/organisms/SectionTitle.vue"
import SectionMovies from "~/components/pages/movies/SectionMovies.vue"
export default {
  components: { SectionTitle, SectionMovies },
  data() {
    return {
      movies: [],
      upcoming: [],
    }
  },
  mounted() {
    this.getMovies()
  },
  methods: {
    async getMovies() {
      try {
        const res = await this.$axios.$get("/movie/popular")
        const res2 = await this.$axios.$get("/movie/upcoming")
        this.movies = res.results.slice(0, 8)
        this.upcoming = res2.results.slice(0, 8)
      } catch (e) {
        this.loading = false
        console.log(e.message);
      }
    }
  }
}
</script>
