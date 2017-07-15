<template>
	<div class="view">
		<div class="card">
			<div class="card-image">
				<figure>
					<a :href="user.html_url" target="blank">
						<img :src="user.avatar_url" :alt="user.login">
					</a>
				</figure>
			</div>

			<div class="card-content">
				<p class="title is-4">
					<a :href="user.html_url">
						{{ user.name }}
					</a>
				</p>
				<p class="subtitle is-6"> 
					{{ user.location }}
					-
					<a :href="user.blog"> {{ user.blog }} </a>
				</p>
				
				<!-- Bio -->
				<div class="content">
					{{ user.bio }}
				</div>
				
				<hr>

				<!-- Repositories -->
				<div class="content repositories">
					<h2 class="title"> Projects </h2>
					
					<!-- Single repository -->
					<a :href="repo.html_url" v-for="repo in repositories" target="blank">
						<div class="card repository">
							<p class="title is-4">
								<a :href="repo.html_url" target="blank"> {{ repo.name }} </a>
							</p>
							
							<span>Stars {{ repo.stargazers_count }} </span>
							<span>Forks {{ repo.forks }} </span>
						</div>
					</a>
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
				repositories: '',
			};
		},
		created() {
			// Get username from url param
			const username = this.$route.params.username;
			// Get a single user
			this.$http.get(`https://api.github.com/users/${username}`)
			.then(
				(user) => {
					const data = JSON.parse(user.bodyText);
					this.user = data;
					console.log(data);
					this.GetRepositories();
				},
				(err) => {
					console.log(err);
				},
			);
		},
		methods: {
			GetRepositories() {
				// Get username from url param
				const username = this.$route.params.username;

				this.$http.get(`https://api.github.com/search/repositories?q=user:${username}`)
				.then(
					(repo) => {
						this.repositories = JSON.parse(repo.bodyText).items;
						console.log(repo.bodyText);
					},
					(err) => {
						console.log(err);
					},
				);
			},
		},
	};
</script>

<style>
	.card{
		width: 460px;
		margin: 0 auto;
	}

	.card .card-image img{
		width: 100%;
	}

	.repositories .repository{
		margin-bottom: 10px;
		padding: 10px;
	}

	.repositories .repository::before{
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 4px;
		background-color: #EEE;
		-webkit-transition: background-color .5s;
		-moz-transition: background-color .5s;
		-ms-transition: background-color .5s;
		transition: background-color .5s;
	}

	.repositories .repository:hover::before{
		background-color: #00d1b2;
	}

	/*.repositories .repository:last-child{
		margin-bottom: 0;
	}*/
</style>
