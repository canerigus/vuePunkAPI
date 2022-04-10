<template>
  <v-app>
      <nav-bar></nav-bar>
      <v-main>
      <v-container style="margin-top:4rem; max-width: 90vw">
        <beer-list></beer-list>
      </v-container>
    </v-main>
    <footer-bar></footer-bar>
  </v-app>
</template>

<script>
import BeerList from './components/BeerList';
import NavBar from './components/NavBar';
import FooterBar from './components/FooterBar';

export default {
  name: 'App',
  components: {
    BeerList,
    NavBar,
    FooterBar
  },
  data: () => ({
    beers: [],
  }),
};
</script>
