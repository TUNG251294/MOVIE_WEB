<template>
  <v-card color="grey darken-4" dark flat tile>
    <v-window v-model="onboarding">
      <v-window-item v-for="movie in sliderMovies" :key="movie.id">
        <v-card color="transparent" height="250" :to="`/movies/${movie.id}`">
          <v-row
            class="fill-height"
            align="center"
            justify="center"
          >
            <v-col sm="5" md="4" lg="3" offset-sm="1">
              <v-card-text class="text-left white--text font-weight-bold display-1">
                {{movie.original_title}}      
              </v-card-text>
              <Rating :data="movie"/>
            </v-col>
            <v-col sm="3" offset-sm="2" offset-md="3">
              <img height="250" :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"/>
            </v-col>
          </v-row>
        </v-card>
      </v-window-item>
    </v-window>

    <v-card-actions class="justify-space-between">
      <v-btn text @click="prev">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-item-group v-model="onboarding" class="text-center" mandatory>
        <v-item
          v-for="n in sliderMovies.length"
          :key="`btn-${n}`"
          v-slot="{ active, toggle }"
        >
          <v-btn :input-value="active" icon @click="toggle">
            <v-icon>mdi-minus-thick</v-icon>
          </v-btn>
        </v-item>
      </v-item-group>
      <v-btn text @click="next">
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import Rating from '~/components/organisms/Rating.vue'
export default {
  components: { Rating },
  props: {
    length: { type: Number }
  },
  data() {
    return {
      onboarding: 0,
      sliderMovies: []
    }
  },
  mounted(){
    this.getSliderMovies()
  },
  methods: {
    async getSliderMovies() {
      await this.$axios
        .$get(`/movie/top_rated`)
        .then((response) => {
          this.sliderMovies = response.results.slice(0,4)
        })
    },
    next() {
      this.onboarding =
        this.onboarding + 1 === this.sliderMovies.length ? 0 : this.onboarding + 1
    },
    prev() {
      this.onboarding =
        this.onboarding - 1 < 0 ? this.sliderMovies.length - 1 : this.onboarding - 1
    }
  }
}
</script>