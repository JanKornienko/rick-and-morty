<template>
	<div class="container">
		<h1 class="title is-1">Rick and Morty Gallery</h1>
		<div v-if="characters != null">
			<div>
				<input class="input is-primary" type="text" placeholder="Search" v-model="search" @input="load(1)">
				<div class="select is-primary">
					<select v-model="status" placeholder="Status">
						<option selected key="0" label="All" value=""></option>
						<option key="1" label="Alive" value="alive"></option>
						<option key="2" label="Dead" value="dead"></option>
						<option key="3" label="Unknown" value="unknown"></option>
					</select>
				</div>
			</div>
			<div class="is-flex is-flex-wrap-wrap is-justify-content-center columns">
				<div class="box column is-one-quarter mx-3" v-for="char in characters.results" :key="char">
					<figure class="image is-128x128">
						<img class="photo" :src="char.image">
					</figure>
					<div>
						<h5 class="title is-5">{{ char.name }}</h5>
						<h5 class="title is-5">{{ char.species }} - {{ char.gender }}</h5>
						<h5 class="title is-5">{{ char.status }}</h5>
						<h5 class="title is-5">{{ char.origin.name }}</h5>
					</div>
				</div>
			</div>
			<div class="pagination">
				<ul class="pagination-list">
					<li class="pagination-link" :class="{ 'is-current': page == currentPage }" v-for="page in characters.info.pages" :key="page" @click="load(page)">{{ page }}</li>
				</ul>
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
				characters: null,
				status: null,
				currentPage: 1,
				search: ''
			};
		},
		created() {
			this.load(1);
		},
		methods: {
			load(page) {
				$.get(this.url, { name: this.search, page: page, status: this.status } , response => {
					this.characters = response;
					this.currentPage = page;
				}).fail(() => {
					this.characters = null;
				});
			}
		},
		watch: {
			status: function() {
				this.load(1);
			}
		}
	};
</script>