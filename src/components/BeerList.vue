<template>
    <v-row class="text-center">
      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Beertiful App
        </h1>
        <v-btn @click="randomBeer"
        color="rgb(54%, 45%, 45%)" elevation="4" x-large outlined> 
          Get Beers
        </v-btn>
<!--         <p class="subheading font-weight-regular" style="margin:1rem" v-for="beer in stuff" :key="beer.id">
         <b> {{beer.id}} {{beer.name}} {{beer.first_brewed}} </b>
        </p>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col class="mb-4">
        <p class="subheading font-weight-regular" style="margin:1rem" v-for="beer in stuff" :key="beer.id">
         <b>{{beer.beer}}</b>
        </p> -->
      </v-col>
    </v-row>
</template>

<script>
  export default {
    name: 'BeerList',
    data: () => ({
    }),
    methods: {
    getBeer(){
      this.stuff.forEach(stuff => {
        console.log(stuff.beer)
      })
    },
    randomBeer(){
        console.time('fetchrandombeer')
        fetch(`https://api.punkapi.com/v2/beers?page=${1}&per_page=80`)
        .then((response) => {
          if (response.ok) {
            console.log('response')
            console.log(response)
            return response.json();
          }
        })
        .then((data) => {
          console.log('data')
          console.log(data)
          console.timeLog()
        console.timeEnd('fetchrandombeer');
        })
        .catch((error) => {
          console.log(error);
          /* this.error = 'Failed to fetch data - please try again later.'; */
        }); 
    }
  }
  }
</script>


/* randomBeer(){
        console.time('fetchrandombeer')
        fetch('https://api.punkapi.com/v2/beers/random')
        .then((response) => {
          if (response.ok) {
            console.log('response')
            console.log(response)
            return response.json();
          }
        })
        .then((data) => {
          console.log('data')
          console.log(data)
          console.timeLog()
        console.timeEnd('fetchrandombeer');
        })
        .catch((error) => {
          console.log(error);
          /* this.error = 'Failed to fetch data - please try again later.'; */
        }); 
    } */