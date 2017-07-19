<template>
	<div class="view">
		
		<loading v-if="!users"></loading>

		<!--<div class="filters">
			<a href="" class="button is-primary">Followers</a>
			<a href="" class="button is-primary">Repositories</a>
			<a href="" class="button is-primary">Joined date</a>
		</div>-->

		<!-- Users box -->
		<article class="users wrap">
			<div class="user card" v-for="user in users">
				<!-- Profile Photo -->
				<div class="card-image">
					<figure class="image is4by3">
						<router-link :to="{ name: 'Profile', params: { username: user.login } }" :title="'Open ' + user.login + '`s profile'">
							<img :src="user.avatar_url" :alt="user.login">
						</router-link>
					</figure>
				</div>

				<!-- Profile Information -->
				<div class="card-content">
					<!-- Name -->
					<h1 class="title is-4">
						<a :href="user.html_url">
							@{{ user.login }}
						</a>
					</h1>
				</div>
			</div>
		</article>

		<!-- Load More Button -->
		<div class="wrap">
			<a class="button is-info" @click="LoadMoreProfiles">Load more profiles</a>
		</div>
	</div>
</template>

<script>
	// Import components
	import Loading from '@/components/Loading';

	export default {
		name: '',
		data() {
			return {
				users: '',
			};
		},
		components: {
			loading: Loading,
		},
		created() {
			this.$http.get('https://api.github.com/search/users?q=location:Angola&per_page=30')
			.then(
				(users) => {
					const data = JSON.parse(users.bodyText);
					this.users = data.items;
					console.log(data);
				},
				(err) => {
					console.log(err);
				},
			);
		},
		methods: {
			LoadMoreProfiles() {
				/*
				*	This method fecth more users from Github API
				*/
				this.$http.get('https://api.github.com/search/users?q=location:Angola&per_page=30&page=2')
				.then(
					(users) => {
						// Parse the raw data as JSON format
						const data = JSON.parse(users.bodyText);
						// Concatenate the fetched results with a existing array
						this.users = this.users.concat(data.items);
						console.log(data);
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
	.wrap{
		width: 70%;
		margin: 0 auto;
	}

	.users{
		overflow: auto;
	}

	.users .user{
		width: 25%;
		min-height: 250px;
		float: left;
		margin-bottom: 15px;
	}

	.users .user .card-content{
		padding: 10px;
	}
</style>
