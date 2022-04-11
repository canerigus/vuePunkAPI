<template>
	<v-toolbar
		dark
		color="#483330"
		style="position: fixed; z-index: 10; width: 100%; top: 0"
		flat
	>
		<v-toolbar-title>Beertiful</v-toolbar-title>
		<v-text-field
			dense
			style=" max-width: 15rem; margin-left:auto;"
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
	</v-toolbar>
</template>
<script>
export default {
	emits: ["beer-search"],
	name: "NavBar",
	data() {
		return {
			beerSearched: "",
		};
	},
	watch: {
			beerSearched() {
				if(this.beerSearched !== null){
			this.$emit("beer-search", this.beerSearched);
			}
		},
	},
	methods: {
		clearSearch() {
			this.beerSearched = "";
		}
	},
};
</script>
