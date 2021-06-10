<template>
	<div class="container">
		<h1 class="title is-1 is-flex is-justify-content-center">Rick and Morty Gallery</h1>
		<div v-if="characters != null">
			<div class="is-flex is-justify-content-center">
				<div class="columns column is-four-fifths">
					<input class="input is-primary column is-four-fifths" type="text" placeholder="Search" v-model="search" @input="load(1)">
					<div class="select is-primary column is-flex is-justify-content-flex-end is-align-items-center">
						<select v-model="status" placeholder="Status">
							<option selected key="0" label="All" value=""></option>
							<option key="1" label="Alive" value="alive"></option>
							<option key="2" label="Dead" value="dead"></option>
							<option key="3" label="Unknown" value="unknown"></option>
						</select>
					</div>
				</div>
			</div>
			<div class="is-flex is-flex-wrap-wrap is-justify-content-center columns mt-5">
				<div class="box is-flex is-flex-direction-column is-justify-content-center is-align-items-center column is-one-quarter mx-3 py-5" v-for="char in characters.results" :key="char">
					<figure class="image is-128x128 mb-3">
						<img class="photo" :src="char.image">
					</figure>
					<div class="is-flex is-flex-direction-column is-align-items-center">
						<h4 class="title is-4 mb-5 has-text-white is-flex-is-justify-content-center">{{ char.name }}</h4>
						<h5 class="subtitle is-5 mb-1 has-text-white">{{ char.species }} - {{ char.gender }}</h5>
						<h5 class="subtitle is-5 mb-1 has-text-white" :class="statusColor(char.status)">{{ char.status }}</h5>
						<h5 class="subtitle is-5 mb-1 has-text-white">{{ char.origin.name }}</h5>
					</div>
				</div>
			</div>
			<div class="pagination is-flex is-justify-content-center">
				<ul class="pagination-list is-flex is-justify-content-center column is-four-fifths">
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
			},
			statusColor(status) {
				switch (status) {
					case 'Alive':
						return 'has-text-success';
					case 'Dead':
						return 'has-text-danger';
					case 'unknown':
						return 'has-text-white';
				};
			}
		},
		watch: {
			status: function() {
				this.load(1);
			}
		}
	};
</script>