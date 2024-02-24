<template>
  <v-container>
    <div class="text-center my-4">
      <v-btn value="popular" class="mr-2" @click="handleGetBy('popular')"
        >Popular</v-btn
      >
      <v-btn value="upcoming" class="mr-2" @click="handleGetBy('upcoming')"
        >Upcoming</v-btn
      >
      <v-btn value="top_rated" class="mr-2" @click="handleGetBy('top_rated')"
        >Top Rated</v-btn
      >
    </div>
    <v-row v-if="$fetchState.pending">
      <v-col cols="12" sm="3" v-for="i in 20" :key="i">
        <v-skeleton-loader type="image, list-item-two-line" />
      </v-col>
    </v-row>
    <v-row v-else-if="$fetchState.error">
      <h2 class="error">An error occurred!</h2>
    </v-row>
        <SectionMovies v-else :movies="movies" />
    <v-row>
      <v-col cols="12">
        <v-pagination
          :total-visible="10"
          v-model="currentPage"
          :length="totalPages"
          @input="handlePageChange"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import SectionMovies from '~/components/pages/movies/SectionMovies.vue'
export default {
  components: { SectionMovies },
  data() {
    return {
      movies: [],
      getBy: "popular",
      totalResults: 1,
      totalPages: 1,
      currentPage:
        parseInt(this.$route.query.page) > 1
          ? parseInt(this.$route.query.page)
          : 1,
    };
  },
  async fetch() {
    await this.$axios
      .$get(`/movie/${this.getBy}?page=${this.currentPage}`)
      .then((response) => {
        this.movies = response.results;
        this.totalResults = response.total_results;
        this.totalPages = response.total_pages;
        this.$router.push({ query: { page: this.currentPage } });
      });
  },
  methods: {
    handlePageChange(value) {
      this.currentPage = value;
      this.$fetch();
    },
    handleGetBy(value) {
      this.getBy = value;
      this.$fetch();
    },
  },
};
</script>
