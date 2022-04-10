<template>
	<v-toolbar
		dark
		color="#221213"
		style="position: fixed; z-index: 10; width: 100%; top: 0"
		flat
	>
		<v-toolbar-title>BEERTIFUL</v-toolbar-title>
		<v-text-field
			style="margin-left: auto; max-width: 30rem"
			v-model="beerSearched"
			hide-details
			single-line
			label="Looking for a beer?"
			type="text"
			clearable
			filled
			@keydown.enter="setBeer"
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
			this.$emit("beer-search", this.beerSearched);
		},
	},
	methods: {
		clearSearch() {
			this.beerSearched = "";
		},
		setBeer(event) {
			this.beerSearched = event.target.value
		},
	},
};
</script>
