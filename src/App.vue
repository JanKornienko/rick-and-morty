<template>
	<h1 class="title is-1">Rick and Morty Gallery</h1>
	<div v-for="char in charactersObject" :key="char">
		<figure class="image is-128x128">
			<img :src="char.image" alt="">
		</figure>
		<h5 class="title is-5">{{ char.name }}</h5>
		<h5 class="title is-5">{{ char.status }}</h5>
		<h5 class="title is-5">{{ char.species }}</h5>
		<h5 class="title is-5">{{ char.gender }}</h5>
		<h5 class="title is-5">{{ char.origin.name }}</h5>
	</div>
</template>

<script>
	import $ from 'jquery';

	export default {
		name: 'App',
		data() {
			return {
				url: 'https://rickandmortyapi.com/api/character',
				charactersObject: []
			};
		},
		created() {
			$.get(this.url, (response) => {
				this.charactersObject = response.results;
				console.log(this.charactersObject);
			});
		}
	};
</script>