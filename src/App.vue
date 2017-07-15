<template>
	<div id="app">
		<header class="main-header">
			<h1>ANGOLANS ON GITHUB</h1>
			<p>Know all the coders based in Angola who loves open source in the Octocat's platform a.k.a Github.</p>
		</header>
		<router-view></router-view>
		<footer class="main-footer">
			<div class="content">
				<p>Released under the <a href="">MIT License</a></p>
				<p>With ❤ by <a href="https://github.com/flowck" title="Hi there, it's me">Firmino Changani</a></p>
			</div>
		</footer>
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

	body, html{
		height: 100%;
		position: relative;
	}

	a{
		-webkit-transition: color .5s;
		-moz-transition: color .5s;
		-ms-transition: color .5s;
		transition: color .5s;
	}

	/* Header */
	.main-header{
		width: 50%;
		margin: 0 auto;
		padding: 20px 0;
		text-align: center;
	}

	/* Footer */
	.main-footer{
		width: 100%;
		padding: 40px 0;
		text-align: center;
		background-color: #34495e;
	}

	.main-footer p,
	.main-footer a{
		color: #FFF;
	}
</style>
