<template>
  <div>
    <v-text-field
      v-model="searchInput"
      @input="debounceSearch"
      append-icon="mdi-magnify"
      placeholder="Search movie here"
      hide-details
      clearable
    />
    <v-expand-transition v-if="searching">
      <p>Searching...</p>
    </v-expand-transition>
    <v-expand-transition v-else-if="!results.length && searchInput">
      <p>No result found</p>
    </v-expand-transition>
    <v-expand-x-transition v-else>
      <v-list color="transparent">
        <v-list-item
          v-for="item in results"
          :key="item.id"
          :to="`/movies/${item.id}`"
        >
          <v-list-item-avatar>
            <v-img
              :src="
                item.poster_path
                  ? `https://image.tmdb.org/t/p/w300${item.poster_path}`
                  : ''
              "
            />
          </v-list-item-avatar>
          <v-list-item-title>
            {{ item.title }}
          </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-expand-x-transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchInput: '',
      results: [],
      debounce: null,
      searching: false
    }
  },
  methods: {
    debounceSearch() {
      if (this.debounce) clearTimeout(this.debounce)
      this.debounce = setTimeout(() => {
        this.getResults(this.searchInput)
      }, 500)
    },
    async getResults(query) {
      this.searching = true
      if (!query) {
        (this.results = []), (this.searching = false)
        return false
      }
      await this.$axios
        .$get(`/search/movie?query=${query}`)
        .then((response) => {
          this.results = response.results.slice(0, 10)
          this.searching = false
        })
        .catch((e) => console.log(e))
    }
  }
}
</script>