<template>
<v-row class="justify-center">
  <v-btn v-if="page>5" fab color="#8C2703" disabled bottom>
		<v-icon color="white">mdi-plus</v-icon>
	</v-btn>
	<v-btn v-else fab color="#8C2703" @click="moreBeer">
		<v-icon color="white">mdi-plus</v-icon>
	</v-btn>
  </v-row>
</template>

<script>
export default {
	name: "LoadBeer",
  props:['page'],
  methods:{
    moreBeer()  {
    this.$emit("more-beer", this.page);
    },
  },
};
</script>
