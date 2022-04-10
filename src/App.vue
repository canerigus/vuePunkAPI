<template>
  <v-app>
    <v-main>
      <nav>
        <nav-bar></nav-bar>
      </nav>
      <v-container>
        <beer-list></beer-list>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import BeerList from './components/BeerList';
import NavBar from './components/NavBar';

export default {
  name: 'App',
  components: {
    BeerList,
    NavBar
  },
  data: () => ({
    beers: [],
  }),
};
</script>
