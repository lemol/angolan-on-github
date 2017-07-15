<template>
	<div id="app">
		<h1>Angolans On Github</h1>
		<hr>
		<router-view></router-view>
	</div>
</template>

<script>
export default {
	name: 'app',
};
</script>

<style>
	/* Global styles */
	@import '../node_modules/bulma/css/bulma.css';

	a{
		-webkit-transition: color .5s;
		-moz-transition: color .5s;
		-ms-transition: color .5s;
		transition: color .5s;
	}
</style>
