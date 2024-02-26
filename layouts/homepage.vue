<template>
  <v-app dark>
    <Navbar />
    <Slider/>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <Footer />
    <v-overlay
      v-model="overlay"
      contained
      opacity="0.95"
      class="align-start justify-end pr-11"
    >
      <div >
        <Search />
        <v-btn color="black" absolute right @click="overlay = false">
          Close
        </v-btn>
      </div>
    </v-overlay>
  </v-app>
</template>

<script>
import Search from '~/components/organisms/Search.vue'
import Footer from '~/components/default/Footer.vue'
import Navbar from '~/components/default/Navbar.vue'
import Slider from '~/components/organisms/Slider.vue'
export default {
  components: { Search, Navbar, Footer, Slider },
  data: () => ({
    overlay: false
  }),
  created() {
    this.$nuxt.$on("openOverlay", ($event) => (this.overlay = $event))
  },
  watch: {
    $route(to, from) {
      this.overlay = false
    }
  }
}
</script>