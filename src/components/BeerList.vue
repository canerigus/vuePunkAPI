<template>
	<v-card elevation="5" class="flex beerCard">
		<v-card-actions>
			<v-btn outlined block class="beerNameButton">{{ beer.name }}</v-btn>
		</v-card-actions>
		<v-row>
			<v-col cols="12">
				<v-card-title primary-title class="beerCardTitle">
					<v-row>
						<v-col cols="6" class="headerColumn">
							First Brewed In: {{ beer.first_brewed }}
						</v-col>
						<v-col cols="6" class="avatarColumn">
							<v-avatar color="#734432" size="75"
								><span class="alcoholVolumeSpan">{{ beer.abv }}</span
								><br />
								<span class="white--text body-1 alcoholPercentageSpan">
									%</span
								></v-avatar
							>
						</v-col>
					</v-row>
				</v-card-title>

				<v-card-text class="text-justify overflow-auto beerCardText">
					<h3>Description:</h3>
					<i>{{ beer.description }}</i>
				</v-card-text>
				<v-list-item>
					<v-list-item-content>
						<v-list-item-title class="font-weight-regular beerCardListItems">
							<b>Goes Well With:</b></v-list-item-title
						>
						<v-list-item-subtitle
							v-for="food in beer.food_pairing"
							:key="food"
							class="font-weight-regular beerCardSubtitles"
						>
							<i>{{ food }}</i>
						</v-list-item-subtitle>
					</v-list-item-content>
				</v-list-item>
			</v-col>
		</v-row>
	</v-card>
</template>

<script>
export default {
	name: "BeerList",
	props: ["beer"],
};
</script>

<style scoped>
.beerCard {
	background-color: #f2ece4;
}

.beerNameButton {
	background-color: #f0a313;
	flex-shrink: 1;
	justify-content: center;
	overflow: hidden;
	font-weight: bold;
	color: #221213;
}
.beerCardText {
	max-height: 150px;
}

.beerCardText > h3 {
	color: #594548;
}
.beerCardText > i {
	color: #221213;
}

.beerCardTitle {
	color: #594548;
}

.beerCardListItems {
	color: #594548;
	text-decoration: underline;
}

.beerCardSubtitles {
	margin: 0.3rem;
	color: #221213;
}

.avatarColumn {
	display: flex;
	justify-content: flex-end;
}
.headerColumn {
	display: flex;
	align-items: center;
}

.alcoholVolumeSpan {
	margin-left: 0.5rem;
	font-size: 1.2rem;
	color: white;
}

.alcoholPercentageSpan {
	margin-left: 0.1rem;
}
</style>
