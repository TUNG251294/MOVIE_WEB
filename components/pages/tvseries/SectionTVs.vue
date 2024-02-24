<template>
  <v-row>
    <v-col cols="6" sm="3" v-for="tvserie in tvseries" :key="tvserie.id">
      <TVSerieCard :tvserie="tvserie" :genres="genres"/>
    </v-col>
  </v-row>
</template>

<script>
import TVSerieCard from '~/components/pages/tvseries/TVSerieCard.vue'
export default {
  components: { TVSerieCard },
  props: {
    tvseries: { type: Array }
  },
  data(){
    return{
      genres: []
    }
  },
  async fetch() {
    await this.$axios
      .$get('/genre/tv/list')
      .then((response) => {
        this.genres = response.genres
      })
      .catch((e)=>{
        console.log(e)
      })
  }
}
</script>