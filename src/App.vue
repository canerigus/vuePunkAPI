<template>
	<v-app>
		<nav-bar @beer-search="searchedBeer"></nav-bar>
		<v-main>
			<v-container style="margin-top: 5rem">
				<beer-list :beerData="beerData"></beer-list>
			</v-container>
			<load-beer
				@more-beer="moreBeer"
				:page="page"
				style="margin-bottom: 5.5rem; margin-top: 1rem"
			></load-beer>
		</v-main>
		<footer-bar style="margin-top: auto"></footer-bar>
	</v-app>
</template>

<script>
import BeerList from "./components/BeerList";
import NavBar from "./components/NavBar";
import FooterBar from "./components/FooterBar";
import LoadBeer from "./components/LoadBeer";
import dummydata from './dummydata';

export default {
	name: "App",
	components: {
		BeerList,
		NavBar,
		FooterBar,
		LoadBeer,
	},
	data: () => ({
		beerData: dummydata,
		cacheBeers: [],
		beerNames: [],
		page: 1,
	}),
	beforeCreate() {
		/* fetch(`https://api.punkapi.com/v2/beers?page=${1}&per_page=80`)
			.then((response) => {
				if (response.ok) {
					return response.json();
				}
			})
			.then((bulkBeerData) => {
				bulkBeerData.forEach((beer) => {
					this.beerData.push({
						id: beer.id,
						name: beer.name,
						first_brewed: beer.first_brewed,
						food_pairing: beer.food_pairing,
						description: beer.description,
						abv: beer.abv,
					});
				});
				this.cacheBeers = structuredClone(this.beerData);
			})
			.catch((error) => {
				console.log(error);
				//no need to catch cause api doesnt return error.
          console.log(error) 
			}); */
	},
	methods: {
		searchedBeer(value) {
			this.beerData = this.cacheBeers;
			this.beerData = this.beerData.filter((beer) => {
				return (beer.name || "").toLowerCase().indexOf(value || "") > -1;
			});
		},
		async moreBeer() {
			await fetch(
				`https://api.punkapi.com/v2/beers?page=${this.page}&per_page=80`
			)
				.then((response) => {
					if (response.ok) {
						return response.json();
					}
				})
				.then((bulkBeerData) => {
					bulkBeerData.forEach((beer) => {
						this.beerData.push({
							id: beer.id,
							name: beer.name,
							first_brewed: beer.first_brewed,
							food_pairing: beer.food_pairing,
							description: beer.description,
							abv: beer.abv,
						});
            this.cacheBeers.push({
							id: beer.id,
							name: beer.name,
							first_brewed: beer.first_brewed,
							food_pairing: beer.food_pairing,
							description: beer.description,
							abv: beer.abv,
						});
					});
					this.page++;
				})
				.catch((error) => {
          //no need to catch cause api doesnt return error.
          console.log(error) 
				});
		},
	},
};
</script>
