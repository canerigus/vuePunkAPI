<template>
	<v-toolbar dark color="#483330" class="footerToolbar">
		<p style="margin: auto">© 2022 - https://github.com/canerigus</p>
	</v-toolbar>
</template>
<script>
export default {
	name: "FooterBar",
};
</script>

<style scoped>
.footerToolbar {
	position: fixed;
	z-index: 10;
	width: 100%;
	bottom: 0;
}

p {
	margin: auto;
}
</style>
