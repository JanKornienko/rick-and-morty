<template>
	<div class="container is-max-desktop is-flex is-flex-direction-column">
		<h1 class="title is-1">Rick and Morty Gallery</h1>
		<a class="button is-primary" @click="prev()">Previous</a>
		<h5 class="title is-5">Page: {{ this.page }}</h5>
		<a class="button is-primary" @click="next()">Next</a>
		<div class="columns" v-for="char in charactersObject" :key="char">
			<div class="column box is-one-fifth">
				<figure class="image is-128x128">
					<img :src="char.image" alt="">
				</figure>
				<h5 class="title is-5">{{ char.name }}</h5>
				<h5 class="title is-5">{{ char.status }} - {{ char.species }}</h5>
				<h5 class="title is-5">{{ char.gender }}</h5>
				<h5 class="title is-5">{{ char.origin.name }}</h5>
			</div>
		</div>
	</div>
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
				charactersObject: [],
				page: 1,
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
				});
			},
			next() {
				if(this.nextUrl != null) {
					this.url = this.nextUrl;
					this.page++;
					this.load();
				};
			},
			prev() {
				if(this.prevUrl != null) {
					this.url = this.prevUrl;
					this.page--;
					this.load();
				};
			}
		}
	};
</script>