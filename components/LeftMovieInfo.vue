<template>
  <v-banner>
    <v-img :src="`https://image.tmdb.org/t/p/w500${data.poster_path}`"> </v-img>
    <div class="text-center mt-2">
      <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="red lighten-1"
            dark
            block
            depressed
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-arrow-right-drop-circle-outline</v-icon>
            <span>Watch Trailer</span>
          </v-btn>
        </template>
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Trailer here
          </v-card-title>
          <v-card-text> This is video </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">
              I accept
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-btn
        color="yellow"
        block
        depressed
        tar
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
    data: { type: Object },
  },
  data() {
    return {
      dialog: false,
    };
  },
};
</script>

<style></style>
