<template>
  <v-toolbar outlined flat class="my-4">
    <v-toolbar-title>{{ title }}</v-toolbar-title>
    <v-spacer></v-spacer>
    <v-btn text color="red" :to="link"
      >See More <v-icon right>mdi-chevron-double-right</v-icon></v-btn
    >
  </v-toolbar>
</template>

<script>
export default {
  props: {
    title: { type: String },
    link: { type: String },
  },
};
</script>

<style></style>
