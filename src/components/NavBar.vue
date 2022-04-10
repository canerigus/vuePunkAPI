<template>
	<v-toolbar
		dark
		color="#221213"
		style="position: fixed; z-index: 10; width: 100%; top: 0"
		flat
	>
		<v-toolbar-title>BEERTIFUL APP</v-toolbar-title>
		<v-spacer></v-spacer>
				<v-btn click="" color="rgb(54%, 45%, 45%)" elevation="4" x-large>
			Load More Beers
		</v-btn>
		<v-spacer></v-spacer>
		<v-btn click="" color="rgb(54%, 45%, 45%)" elevation="4" x-large>
			Random Beer
		</v-btn>
		<v-spacer></v-spacer>
		<v-autocomplete
			rounded
			flat
			hide-no-data
			hide-details
			label="Looking for a beer?"
			solo-inverted
		></v-autocomplete>
	</v-toolbar>
</template>
<script>
export default {
	name: "NavBar",
	data() {
		return {
			isSearch: false,
		};
	},
	methods: {
		toggleIsSearch() {
			this.isSearch = !this.isSearch;
		},
	},
};
</script>
