<template>
  <v-app>
      <nav-bar @beer-search="searchedBeer"></nav-bar>
      <v-main>
      <v-container style="margin-top:5rem; margin-bottom:5rem; max-width: 90vw">
        <beer-list :beerData="beerData"></beer-list>
      </v-container>
    </v-main>
    <footer-bar style="margin-top:auto;"></footer-bar>
  </v-app>
</template>

<script>
import BeerList from './components/BeerList';
import NavBar from './components/NavBar';
import FooterBar from './components/FooterBar';
import dummybeer from "./dummydata";

export default {
  name: 'App',
  components: {
    BeerList,
    NavBar,
    FooterBar
  },
  data: () => ({
    beerData: dummybeer,
  }),
  mounted (){

  },
  methods:{
    searchedBeer(value) {
    this.beerData = dummybeer
    this.beerData = this.beerData.filter(beer => {
      return (beer.name|| '').toLowerCase().indexOf((value || '').trim().toLowerCase()) > -1
    })
    },
  },

};
</script>
