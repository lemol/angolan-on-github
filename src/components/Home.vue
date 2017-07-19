<template>
	<div class="view">
		
		<loading v-if="showLoadBar"></loading>

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
				totalUsers: 0,
				pagination: 0,
				pageNumber: 1,
				showLoadBar: true,
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
					// Number of users found
					this.totalUsers = data.total_count;
					// Pagination is the result of total users found devided by
					// the number of users listed per request which is 30
					this.pagination = Math.round(data.total_count / 30) + 1;
					// Store all the users found
					this.users = data.items;

					// Hide loading bar
					this.showLoadBar = false;
					console.log(this.pagination);
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

				// Show loading bar
				this.showLoadBar = true;

				// Condition to change pagination number
				if (this.pageNumber <= this.pagination) {
					// Increment in the number of the page to be requested on github
					this.pageNumber += 1;

					// Request the data
					this.$http.get(`https://api.github.com/search/users?q=location:Angola&per_page=30&page=${this.pageNumber}`)
					.then(
						(users) => {
							// Parse the raw data as JSON format
							const data = JSON.parse(users.bodyText);
							// Concatenate the fetched results with a existing array
							this.users = this.users.concat(data.items);
							console.log(data);
							// Hide loading bar
							this.showLoadBar = false;
						},
						(err) => {
							console.log(err);
						},
					);
				}
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
