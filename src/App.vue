<script>
import { ref } from 'vue';
import searchForm from './components/searchForm.vue';

export default {
  components:{
    searchForm,
  	},
    name: 'SearchAnime',
    created() {
        this.getAnimeCharacters()
    },
    data() {
        return {
		animeList: [],
        hasError: false,
        searchAnime: ''
        }},
    methods: {
    getAnimeCharacters() {
    axios.get('https://api.jikan.moe/v4/characters?page=0&limit=15&q=&order_by=favorites&sort=desc')
    .then((response) => {
    console.log(response.data)
    this.animeList = response.data
    })
    .catch((error) => {
    console.log(error)
     })
     }
    }}
</script>

<template>
	<div class="app">
		<header>
			<h1>Поиск<strong>Аниме</strong></h1>
			<form class="search-box" @submit.prevent="HandleSearch">
				<input 
					type="search" 
					class="search-field" 
					placeholder="Введите название аниме"
					required
					v-model="search_query" />
			</form>
		</header>
		<main>
			<div class="cards" v-if="animeList.length > 0">
				<Card 
					v-for="anime in animeList" 
					:key="anime.mal_id"
					:anime="anime" />
			</div>
			<div class="no-results" v-else>
				<h3>Ничего не найдено</h3>
			</div>
		</main>
	</div>
</template>


<style lang="scss">
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Fira Sans', sans-serif;
  background-color: #1ec028;
}
a {
	text-decoration: none;
}
header {
	padding-top: 50px;
	padding-bottom: 50px;
	h1 {
		color: #0b0909;
		font-size: 42px;
		font-weight: 400;
		text-align: center;
		text-transform: uppercase;
		margin-bottom: 30px;
		strong {
			color: #0b0909;
		}
		&:hover {
			color: #040202;
		}
	}
	.search-box {
		display: flex;
		justify-content: center;
		padding-left: 30px;
		padding-right: 30px;

		.search-field {
      		display: block;
			width: 100%;
			max-width: 600px;
			padding: 15px;
			appearance: none;
			background: none;
			border: none;
			outline: none;
			background-color: #F3F3F3;
			box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

			border-radius: 8px;
			color: #020202;
			font-size: 20px;
			transition: 0.4s;
			&::placeholder {
				color: #AAA;
			}
			&:focus, &:valid {
				color: #FFF;
				background-color: #313131;
				box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
			}
		}
	}
}

main {
	max-width: 1200px;
	margin: 0 auto;
	padding-left: 30px;
	padding-right: 30px;

	.searchForm {
	display: flex;
	flex-wrap: wrap;
	margin: 0 -8px;
	}

	h3{
		font-size: 20px;
		display: flex;
		justify-content: center;
		padding-left: 30px;
		padding-right: 30px;
	}

}
</style>
