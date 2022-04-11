<template>
	<v-app>
		<nav-bar @beer-search="searchedBeer"></nav-bar>
		<v-main >
			<v-container style="margin-top: 5rem">
				<v-progress-circular
				style="position: fixed; z-index: 10; top: 50%; left: 50%; margin-top: -6rem; margin-left: -2rem;"
				v-if="isLoading"
				:size="70"
				:width="7"
				color="#8C2703"
				indeterminate
    ></v-progress-circular>
				<v-row class="justify-center" v-else>
					<v-col
						cols="10"
						md="6"
						xl="4"
						class="pa-3 d-flex flex-column"
						v-for="beer in beerData"
						:key="beer.id"
					>
						<beer-list :beer="beer"></beer-list>
					</v-col>
				</v-row>
			</v-container>
			<load-beer
				@more-beer="moreBeer"
				v-if="!isLoading"
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

export default {
	name: "App",
	components: {
		BeerList,
		NavBar,
		FooterBar,
		LoadBeer,
	},
	data: () => ({
		beerData: [],
		cacheBeers: [],
		page: 2,
		isLoading: false,
	}),
	async mounted() {
		//fetch first page of beers at entry
		this.isLoading = true;
		await fetch(`https://api.punkapi.com/v2/beers?page=${1}&per_page=80`)
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
				//create cache for filtering. needs to be a deep clone.
				this.cacheBeers = structuredClone(this.beerData);
				this.isLoading = false;
			})
			.catch((error) => {
				console.log(error);
			});
	},
	methods: {
		searchedBeer(value) {
			//filter beers based on search value emitted from NavBar textfield. Then display on screen accordingly.
			//display matching results of searched value with Beer name. 
			this.beerData = this.cacheBeers.filter((beer) => {
				return (beer.name || "").toLowerCase().indexOf(value || "") > -1;
			});
		},
		async moreBeer() {
			//fetch more beers until the limit is reached. limit is page=5_id=325. After that disable the button.
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
						//discard duplicate entries. push new beers.
						if (
							!this.beerData.some((existingBeer) => existingBeer.id === beer.id)
						) {
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
						}
					});

					this.page++;
				})
				.catch((error) => {
					console.log(error);
				});
		},
	},
};
</script>
