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
						style="
							background-color: #f0a313;
							flex-shrink: 1;
							justify-content: start;
							overflow: hidden;
						"
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
							style="max-height: 150px"
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
export default {
	name: "BeerList",
	props: ["beerData"],
};
</script>
