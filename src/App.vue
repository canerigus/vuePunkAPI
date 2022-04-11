<template>
	<v-app>
		<nav-bar @beer-search="searchedBeer" @beer-filter="filteredBeer"></nav-bar>
		<v-main>
			<v-container class="appbarContainer">
				<v-progress-circular
					class="circularLoop"
					v-if="isLoading"
					:size="70"
					:width="7"
					color="#8C2703"
					indeterminate
				></v-progress-circular>
				<div v-else>
					<v-row
						class="justify-center"
						v-if="filteredBeers.length > 0 || searchQuery.length > 0"
					>
						<v-col
							cols="10"
							md="6"
							xl="4"
							class="pa-3 d-flex flex-column"
							v-for="beer in filteredBeers"
							:key="beer.id"
						>
							<beer-list :beer="beer"></beer-list>
						</v-col>
					</v-row>
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
				</div>
			</v-container>
			<load-beer
				@more-beer="moreBeer"
				v-if="!isLoading"
				:page="page"
				class="loadbeerComponent"
			></load-beer>
		</v-main>
		<footer-bar class="footerComponent"></footer-bar>
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
	data() {
		return {
			beerData: [],
			filteredBeers: [],
			searchQuery: "",
			trigger: 0,
			page: 2,
			isLoading: false,
			volume: 0,
			operator: "",
		};
	},
	mounted() {
		//fetch first page of beers at entry
		this.initialData();
	},
	watch: {
		trigger() {
			if (
				this.searchQuery.length > 0 ||
				(this.volume > 0 && this.operator.length > 0)
			) {
				this.filteredBeers = this.beerData.filter((beer) => {
					if (this.operator === "more") {
						return (
							(beer.name || "")
								.toLowerCase()
								.indexOf(this.searchQuery.toLowerCase() || "") > -1 &&
							beer.abv > this.volume
						);
					}
					if (this.operator === "less") {
						return (
							(beer.name || "")
								.toLowerCase()
								.indexOf(this.searchQuery.toLowerCase() || "") > -1 &&
							beer.abv < this.volume
						);
					} else {
						return (
							(beer.name || "")
								.toLowerCase()
								.indexOf(this.searchQuery.toLowerCase() || "") > -1
						);
					}
				});
			} else if (
				this.searchQuery.length > 0 ||
				(this.volume === 0 && this.operator.length === 0)
			) {
				this.filteredBeers = this.beerData.filter((beer) => {
					return (
						(beer.name || "")
							.toLowerCase()
							.indexOf(this.searchQuery.toLowerCase() || "") > -1
					);
				});
			} else {
				//handle non-matching cases
				return this.filteredBeers;
			}
		},
	},
	methods: {
		searchedBeer(search) {
			this.searchQuery = search;
			this.trigger++;
		},
		filteredBeer(vol) {
			this.trigger++;
			if (vol === "cleared") {
				this.operator = "";
				this.volume = 0;
				return;
			}
			let numberRegex = /\d+/;
			let percentMatch = vol.match(numberRegex);
			let percentVolume = parseInt(percentMatch[0]);
			this.volume = percentVolume;

			if (vol.includes("less")) {
				this.operator = "less";
			}
			if (vol.includes("more")) {
				this.operator = "more";
			}
		},
		async initialData() {
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
					//create cache for filtering.
					this.isLoading = false;
				})
				.catch((error) => {
					console.log(error);
				});
		},
		async moreBeer() {
			this.isLoading = true;
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
						}
					});
					this.page++;
					this.isLoading = false;
				})
				.catch((error) => {
					console.log(error);
				});
		},
	},
};
</script>

<style scoped>
.appbarContainer {
	margin-top: 5rem;
}

.circularLoop {
	position: fixed;
	z-index: 10;
	top: 50%;
	left: 50%;
	margin-top: -6rem;
	margin-left: -2rem;
}

.loadbeerComponent {
	margin-bottom: 5.5rem;
	margin-top: 1rem;
}

.footerComponent {
	margin-top: auto;
}
</style>
