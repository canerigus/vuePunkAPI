<template>
	<v-toolbar dark color="#483330" class="navbarToolbar" flat>
		<v-row class="navbarHeaderRow">
			<v-col>
				<v-toolbar-title>Beertiful</v-toolbar-title>
			</v-col>
			<v-col class="filterWrapper">
				<v-select
					label="Alcohol by Volume"
					v-model="input"
					:items="alcoholVolumes"
					dense
					clear-icon="mdi-close-circle"
					clearable
					outlined
					class="filterWrapperSelect pa-3 mt-7"
					@change="changeSelectedVolume"
				></v-select>
				<v-text-field
					dense
					class="filterWrapperTextfield pa-3"
					v-model="beerSearched"
					hide-details
					background-color="#6e5a57"
					color="white"
					outlined
					label="Looking for a beer?"
					type="text"
					clearable
					filled
					clear-icon="mdi-close-circle"
					@click:clear="clearSearch"
				></v-text-field>
			</v-col>
		</v-row>
	</v-toolbar>
</template>
<script>
export default {
	emits: ["beer-search", "beer-filter"],
	name: "NavBar",
	data() {
		return {
			beerSearched: "",
			alcoholVolumes: ["less than 5%", "less than 10%", "more than 10%"],
			selectedVolume: "",
			input: "",
		};
	},
	watch: {
		beerSearched() {
			if (this.beerSearched !== null) {
				this.$emit("beer-search", this.beerSearched);
			}
		},
		selectedVolume() {
			this.$emit("beer-filter", this.selectedVolume);
		},
	},
	methods: {
		clearSearch() {
			this.beerSearched = "";
		},
		changeSelectedVolume() {
			if (this.input === null) {
				return (this.selectedVolume = "cleared");
			}
			this.selectedVolume = this.input;
		},
	},
};
</script>

<style scoped>
.navbarToolbar {
	position: fixed;
	z-index: 10;
	width: 100%;
	top: 0;
}

.navbarHeaderRow {
	display: flex;
	justify-content: center;
	align-items: center;
}

.filterWrapper {
	display: flex;
	flex-direction: row;
	justify-content: space-evenly;
	align-items: center;
}

.filterWrapperSelect {
	flex: 1;
}
.filterWrapperTextfield {
	flex: 1;
}
</style>
