<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="3">
        <left-movie-info :data="data" />
      </v-col>
      <v-col cols="12" sm="9">
        <div class="d-md-flex align-center">
          <h1 class="display-1 font-weight-bold">
            {{ data.original_title }}
          </h1>
          <span class="display-1 font-weight-light">
            {{ data.release_date.slice(0, 4) }}
          </span>
          <v-spacer></v-spacer>
          <rating :data="data" />
        </div>
        <social-share />
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
            <h3
              class="pink--text font-weight-bold title"
              v-if="recommendations.length"
            >
              Recommentation
            </h3>
            <v-slide-group multiple show-arrows>
              <v-slide-item
                v-for="(image, index) in recommendations.results"
                :key="index"
                v-slot="{ toggle }"
              >
                <v-card @click="toggle" class="ma-4">
                  <v-img
                    cover
                    width="100"
                    height="150"
                    :src="`https://image.tmdb.org/t/p/w500${image.backdrop_path}`"
                  />
                </v-card>
              </v-slide-item>
            </v-slide-group>
          </v-col>
          <v-col cols="12" sm="4">
            <right-movie-info :data="data" />
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import LeftMovieInfo from "~/components/LeftMovieInfo.vue";
import Rating from "~/components/Rating.vue";
import RightMovieInfo from "~/components/RightMovieInfo.vue";
import SocialShare from "~/components/SocialShare.vue";
export default {
  components: { LeftMovieInfo, Rating, SocialShare, RightMovieInfo },
  async asyncData({ params, $axios }) {
    try {
      const res = await $axios.$get(
        `/movie/${params.id}?append_to_response=credits,videos,images`
      );
      const res2 = await $axios.$get(`/movie/${params.id}/recommendations`);
      console.log(res2);
      return {
        data: res,
        recommendations: res2,
      };
    } catch (e) {
      console.log(e.message);
      return {
        data: [],
        recommendations: [],
      };
    }
  },
  computed: {
    getCasts() {
      let casts = [];
      for (const item of this.data.credits.cast) {
        casts.push(item);
      }
      return casts;
    },
  },
  methods: {
    getCastAvatar(profile_path) {
      if (profile_path) {
        return `https://image.tmdb.org/t/p/w45${profile_path}`;
      }
      return "https://upload.wikimedia.org/wikipedia/commons/thumb/5/59/User-avatar.svg/1200px-User-avatar.svg.png";
    },
  },
};
</script>

<style></style>
