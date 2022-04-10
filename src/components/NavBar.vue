<template>
  <v-card color="grey lighten-4" flat height="auto" tile>
    <v-toolbar dark color="#251613">
      <v-toolbar-title >Beertiful</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      <v-autocomplete
        rounded
        flat
        hide-no-data
        hide-details
        label="Looking for a beer?"
        solo-inverted
      ></v-autocomplete>
    </v-toolbar>
  </v-card>
</template>
<script>
export default {
  name: "NavBar",
  data() {
    return {
      isSearch: false,
    };
  },
  methods: {
    toggleIsSearch() {
      this.isSearch = !this.isSearch;
    },
  },
};
</script>

<style scoped>
 .navbar-autocomplete{
   float:right; 
}
</style>

    <v-autocomplete
      v-model="select"
      :loading="loading"
      :items="items"
      :search-input.sync="search"
      cache-items
      class="ms-16"
      flat
      width="20%"
      hide-no-data
      hide-details
      label="What state are you from?"
      solo-inverted
    ></v-autocomplete>