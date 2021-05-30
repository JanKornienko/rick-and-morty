<template>
	<h1 class="title is-1">Rick and Morty Gallery</h1>
	<div v-for="char in charactersObject" :key="char">
		<figure class="image is-128x128">
			<img :src="char.image" alt="">
		</figure>
		<h5 class="title is-5">{{ char.name }}</h5>
		<h5 class="title is-5">{{ char.status }} - {{ char.species }}</h5>
		<h5 class="title is-5">{{ char.gender }}</h5>
		<h5 class="title is-5">{{ char.origin.name }}</h5>
	</div>
	<a class="button is-primary" @click="prev()">Previous</a>
	<a class="button is-primary" @click="next()">Next</a>
</template>

<script>
	import $ from 'jquery';

	export default {
		name: 'App',
		data() {
			return {
				url: 'https://rickandmortyapi.com/api/character',
				nextUrl: '',
				prevUrl: '',
				charactersObject: []
			};
		},
		created() {
			this.load();
		},
		methods: {
			load() {
				$.get(this.url, (response) => {
					this.charactersObject = response.results;
					this.nextUrl = response.info.next;
					this.prevUrl = response.info.prev;
					console.log(this.charactersObject);
					console.log(this.nextUrl);
				});
			},
			next() {
				if(this.nextUrl != null) {
					this.url = this.nextUrl;
					this.load();
				};
			},
			prev() {
				if(this.prevUrl != null) {
					this.url = this.prevUrl;
					this.load();
				};
			}
		}
	};
</script>