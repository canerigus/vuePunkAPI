<template>
	<v-row class="justify-center">
		<v-col
			cols="10"
			md="6"
			xl="4"
			class="pa-3 d-flex flex-column"
			v-for="beer in beerData"
			:key="beer.id"
		>
			<v-card elevation="5" class="flex" style="background-color: #f2ece4">
				<v-card-actions>
					<v-btn
						outlined
						block
						color="#221213"
						style="background-color: #f0a313"
						class="font-weight-bold"
						>{{ beer.name }}</v-btn
					>
				</v-card-actions>
				<v-row>
					<v-col cols="12">
						<v-card-title primary-title style="color: #594548">
							First Brewed In: {{ beer.first_brewed }}
						</v-card-title>
						<v-card-text
							class="text-justify overflow-auto"
							style="max-height: 120px"
						>
							<h3 style="color: #594548">Description:</h3>
							<i style="color: #221213">{{ beer.description }}</i>
						</v-card-text>
						<v-list-item>
							<v-list-item-content>
								<v-list-item-title
									class="font-weight-regular"
									style="color: #594548; text-decoration: underline"
								>
									<b>Goes Well With:</b></v-list-item-title
								>
								<v-list-item-subtitle
									v-for="food in beer.food_pairing"
									:key="food"
									class="font-weight-regular"
									style="margin: 0.3rem; color: #221213"
								>
									<i>{{ food }}</i>
								</v-list-item-subtitle>
							</v-list-item-content>
						</v-list-item>
					</v-col>
				</v-row>
			</v-card>
		</v-col>
	</v-row>
</template>

<script>
import dummybeer from "../dummydata";
export default {
	name: "BeerList",
	data: () => ({
		beerData: dummybeer,
	}),
	methods: {
		/*     getBeer(){
      this.stuff.forEach(stuff => {
        console.log(stuff.beer)
      })
    }, */
		randomBeer() {
			console.time("fetchrandombeer");
			fetch(`https://api.punkapi.com/v2/beers?page=${1}&per_page=${20}`)
				.then((response) => {
					if (response.ok) {
						console.log("response");
						console.log(response);
						return response.json();
					}
				})
				.then((data) => {
					console.log("data");
					console.log(data);
					console.timeLog();
					data.forEach((beer) => {
						this.beerData.push({
							id: beer.id,
							name: beer.name,
							first_brewed: beer.first_brewed,
							food_pairing: beer.food_pairing,
							description: beer.description,
						});
					});
					console.timeEnd("fetchrandombeer");
					console.log("this.beerData");
					console.log(this.beerData);
				})
				.catch((error) => {
					console.log(error);
					/* this.error = 'Failed get beers - please try again later.'; */
				});
		},
	},
};
</script>
