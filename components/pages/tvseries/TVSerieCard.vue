<template>
  <v-card :to="`/tvseries/${tvserie.id}`">
    <v-img
      cover
      :src="`https://image.tmdb.org/t/p/w500${tvserie.poster_path}`"
    ></v-img>
    <v-card-title class="break-word">{{ tvserie.name }}</v-card-title>
    <v-card-text>
      <v-row no-gutters>
        <v-rating
          half-increments
          length="5"
          size="16"
          color="yellow"
          dense
          readonly
          :value="tvserie.vote_average / 2"
        ></v-rating>
        <span class="grey--text text-lighten-1 text-caption mx-2 mt-1">{{
          tvserie.vote_average
        }}</span>
      </v-row>
      <v-row>
        <v-col cols="12">
          <v-chip v-for="genre in tvserie.genre_ids" :key="genre" label small color="pink" class="white--text mr-1 mb-1">
            {{genreTypeName(genre)}}
          </v-chip>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    tvserie: {
      type: Object,
      required: true
    },
    genres: {
      type: Array,
      required: false
    }
  },
  methods: {
    genreTypeName(genreId){
      for(const genre of this.genres){
        if(genre.id == genreId){
          return genre.name
        }
      }
    }
  }
}
</script>