<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="3">
        <LeftMovieInfo :data="data" />
      </v-col>
      <v-col cols="12" sm="9">
        <div class="d-md-flex align-center">
          <h1 class="display-1 font-weight-bold">
            {{ data.original_title }}
          </h1>
          <v-spacer></v-spacer>
          <Rating :data="data" />
        </div>
        <SocialShare />
        <v-divider class="my-4" />
        <v-row>
          <v-col cols="12" sm="8">
            <h3 class="pink--text font-weight-bold title">Overview</h3>
            <!-- class title can not change size by tags-->
            <p>{{ data.overview }}</p>
            <h3 class="pink--text font-weight-bold title">Images</h3>
            <v-slide-group multiple show-arrows>
              <v-slide-item
                v-for="(image, index) in data.images.backdrops"
                :key="index"
                v-slot="{ toggle }"
              >
                <v-card @click="toggle" class="ma-4">
                  <v-img
                    cover
                    width="100"
                    height="150"
                    :src="`https://image.tmdb.org/t/p/w500${image.file_path}`"
                  />
                </v-card>
              </v-slide-item>
            </v-slide-group>
            <h1 class="grey--text title">Cast</h1>
            <v-responsive class="pa-3">
              <v-virtual-scroll height="240" item-height="48" :items="getCasts">
                <template v-slot:default="{ item }">
                  <v-list-item>
                    <v-list-item-avatar>
                      <v-img :src="getCastAvatar(item.profile_path)" />
                    </v-list-item-avatar>
                    <v-list-item-title>
                      {{ item.name }}
                    </v-list-item-title>
                    <v-list-item-subtitle>
                      {{ item.character }}
                    </v-list-item-subtitle>
                  </v-list-item>
                </template>
              </v-virtual-scroll>
            </v-responsive>
          </v-col>
          <v-col cols="12" sm="4">
            <RightMovieInfo :data="data" />
          </v-col>
          <h3 class="pink--text font-weight-bold title">
            Movies Recommentation
          </h3>
          <v-row class="mt-2">
            <v-col
              cols="3"
              sm="3"
              v-for="recommend in recommendations"
              :key="recommend.id"
            >
              <v-card :to="`/movies/${recommend.id}`">
                <v-img
                  :src="`https://image.tmdb.org/t/p/w300${recommend.poster_path}`"
                />
              </v-card>
            </v-col>
          </v-row>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Rating from '~/components/organisms/Rating.vue'
import RightMovieInfo from '~/components/pages/movies/RightMovieInfo.vue'
import SocialShare from '~/components/organisms/SocialShare.vue'
import LeftMovieInfo from '~/components/pages/movies/LeftMovieInfo.vue'
export default {
  head() {
    return {
      title: 'Movie'
    }
  },
  components: { LeftMovieInfo , Rating, SocialShare, RightMovieInfo},
  async asyncData({ params, $axios, error }) {
    try {
      const res = await $axios.$get(
        `/movie/${params.id}?append_to_response=credits,videos,images`
      )
      const res2 = await $axios.$get(`/movie/${params.id}/recommendations`)
      return {
        data: res,
        recommendations: res2.results.slice(0,4)
      }
    } catch (e) {
      console.log(e.message)
      error(e)
    }
  },
  computed: {
    getCasts() {
      let casts = []
      for (const item of this.data.credits.cast) {
        casts.push(item)
      }
      return casts
    }
  },
  methods: {
    getCastAvatar(profile_path) {
      if (profile_path) {
        return `https://image.tmdb.org/t/p/w45${profile_path}`
      }
      return "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/User-avatar.svg/1200px-User-avatar.svg.png"
    }
  }
}
</script>
