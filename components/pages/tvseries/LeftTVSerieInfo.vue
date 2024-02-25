<template>
  <v-banner>
    <v-img :src="`https://image.tmdb.org/t/p/w500${data.poster_path}`"> </v-img>
    <div class="text-center mt-2">
      <v-dialog v-if="getTrailer" v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="red lighten-1"
            dark
            block
            depressed
            v-bind="attrs"
            v-on="on"
            @click="openYoutubeModal"
          >
            <v-icon>mdi-arrow-right-drop-circle-outline</v-icon>
            <span>Watch Trailer</span>
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="headline">{{ data.name }}</span>
          </v-card-title>
          <v-card-text>
              <v-row>
                <v-col cols="12">
                  <div class="iframe-container">
                    <iframe allowfullscreen :src="getTrailer" v-if="isVideo" />
                  </div>
                </v-col>
              </v-row>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="error" text @click="closeModal"> Close </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-btn
        color="yellow"
        block
        depressed
        v-if="data.homepage"
        :href="data.homepage"
        class="mt-2"
      >
        <v-icon>mdi-home</v-icon>
        <span>Home Page</span>
      </v-btn>
    </div>
  </v-banner>
</template>

<script>
export default {
  props: {
    data: { type: Object }
  },
  data() {
    return {
      dialog: false,
      isVideo: false,
    }
  },
  computed: {
    getTrailer() {
      if (!this.data.videos) return;
      const video = this.data.videos.results.find((e) => {
        return e.type === 'Trailer'
      })
      if(video){
        return 'https://www.youtube.com/embed/' + video.key
      }
    }
  },
  methods: {
    closeModal() {
      this.dialog = false
      this.isVideo = false
    },
    openYoutubeModal() {
      this.isVideo = true
    }
  }
}
</script>
