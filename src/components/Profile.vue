<template>
	<div class="view">
		<div class="card">
			<div class="card-image">
				<figure>
					<img :src="user.avatar_url" :alt="user.login">
				</figure>
			</div>

			<div class="card-content">
				<h1 class="title"></h1>
				
				<!-- Bio -->
				<div class="content">
					
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'Profile',
		data() {
			return {
				user: '',
			};
		},
		created() {
			const username = this.$route.params.username;
			// Get a single user
			this.$http.get(`https://api.github.com/users/${username}`)
			.then(
				(user) => {
					const data = JSON.parse(user.bodyText);
					this.user = data;
					console.log(data);
				},
				(err) => {
					console.log(err);
				},
			);
		},
	};
</script>

<style>
	.card{
		width: 460px;
		margin: 0 auto;
	}
</style>
