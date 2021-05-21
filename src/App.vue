<template>
	<div class="app">
		<nav>
			<div class="container">
				<div class="navitems">
					<div class="where">Where in the world?</div>
					<div class="theme">Dark Mode</div>
				</div>
				<div v-if="homePage" class="undernav">
					<div class="searchbar">
						<input
							v-model="search"
							v-on:input="updateSearchList"
							class="search"
							type="text"
							placeholder="Search for a country..."
						/>

						<div class="filtering">
							<div class="filter-dropdown" v-if="!search" @click="filterbtn = !filterbtn">
								<div class="filter">Filter by Region</div>
								<div class="button">
									<div class="line1"></div>
									<div class="line2"></div>
								</div>
							</div>

							<div v-if="filterbtn && !search" class="regionmenu">
								<div @click="changeRegion('ALL')">Africa</div>
								<div @click="changeRegion('Americas')">America</div>
								<div @click="changeRegion('Asia')">Asia</div>
								<div @click="changeRegion('Europe')">Europe</div>
								<div @click="changeRegion('Oceania')">Oceania</div>
							</div>
						</div>
					</div>
					<div class="list" v-if="search && homePage">
						<ul>
							<li class="listmenu" v-for="country in filteredCountries" :key="country.id">
								<div @click="changePage(country)" href="/"><img :src="country.flag" />{{ country.name }}</div>
							</li>
						</ul>
					</div>
				</div>
				<div v-else @click="homePage = !homePage" class="backbtn">Back</div>
				<div class="singlecountry" v-if="!homePage" :v-bind="page">
					<img :src="page.flag" />
					<h2>{{ page.name }}</h2>
					<p>
						<b> Native Name:</b>
						{{ page.name }}
					</p>
					<p><b> Population: </b> {{ page.population }}</p>
					<p><b> Region: </b> {{ page.region }}</p>
					<p><b> Sub Region: </b> {{ page.subregion }}</p>
					<p><b> Capital: </b> {{ page.capital }}</p>
					<br />
					<br />
					<p><b> Top level Domain: </b> {{ page.topLevelDomain[0] }}</p>
					<p><b> Currencies: </b> {{ page.currencies[0].name }}</p>
					<p><b> Language: </b> {{ page.languages[0].name }}</p>
					<p><b> Border Countries: </b> {{ page.languages[0].name }}</p>
				</div>
			</div>
		</nav>

		<!-- <div class="countrylist"> -->
		<ul class="countrylist" v-if="homePage">
			<div class="countrycontainer">
				<li class="country" v-for="country in currentCountries" :key="country.id">
					<img :src="country.flag" />
					<div class="textback">
						<h2>{{ country.name }}</h2>
						<div class="textctn">
							<p>Population: {{ country.population }}</p>
							<p>Region: {{ country.region }}</p>
							<p>Capital: {{ country.capital }}</p>
						</div>
					</div>
				</li>
			</div>
		</ul>
		<!-- </div> -->
	</div>
</template>

<script>
import axios from 'axios';

export default {
	data: function() {
		return {
			currentRegion: 'ALL',
			filterbtn: false,
			all: null,
			currentCountries: null,
			search: '',
			countrylist: null,
			homePage: true,
			page: null,
			liston: false,
			filteredCountries: null,
		};
	},

	mounted() {
		axios.get('https://restcountries.eu/rest/v2/all').then((response) => {
			this.all = response.data;
			this.currentCountries = this.all;
		});
	},
	methods: {
		changeRegion(region) {
			this.currentRegion = region;
			console.log(this.all);

			if (region == 'ALL') {
				this.currentCountries = this.all;
				return;
			}

			this.currentCountries = this.all.filter((country) => {
				return country.region === region;
			});
		},
		changePage(page) {
			this.homePage = false;
			this.page = page;
			console.log(this.page);
		},
		updateSearchList() {
			return (this.filteredCountries = this.all.filter((list) => {
				return list.name.toLowerCase().startsWith(this.search);
			}));
		},
	},
};
</script>

<style>
* {
	margin: 0;
	padding: 0;
}
:root {
	color: white;
	background-color: rgb(1, 35, 51);
	font-family: 'Helvetica Neue', Helvetica, sans-serif;
}

nav {
	width: 100%;
	height: 44.2%;
	top: 0;
	position: sticky;
	display: flex;
}
.container {
	width: 100%;
	background-color: rgb(1, 35, 51);
	margin-bottom: 2em;
}
.navitems {
	display: flex;
	justify-content: space-between;
	align-items: center;
	background-color: rgb(30, 81, 105);
	padding: 2rem 3.5rem;
}

.where {
	font-weight: bold;
	font-size: 1rem;
}
.theme {
	font-size: 1rem;
}
.searchbar {
	margin: 20px;
	align-items: center;
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	justify-content: space-between;
	height: 5rem;
}
.search {
	height: 1.5rem;
	/* width: 50%; */
	background-color: rgba(52, 71, 83, 1);
	width: 100%;
	text-align: start;
	border: 1px rgba(255, 255, 255, 0) solid;
	border-radius: 6px;
	font-size: 0.7rem;
	outline: none;
	color: aliceblue;
	padding: 10px 15px;
}

.search:hover {
	border: 1px white solid;
}
::placeholder {
	color: white;
}

.filtering {
	display: flex;
	flex-direction: column;
	margin-top: 10px;
}

.filter-dropdown {
	display: flex;
	justify-content: space-between;
	align-items: center;
	background-color: rgba(52, 71, 83, 1);
	padding: 10px;
	border-radius: 6px;
}

.filter {
	font-size: 0.9rem;
	/* margin-bottom: 10px; */
}
.button {
	margin-left: 15px;
	display: flex;
}
.line1 {
	margin-right: 4px;
	background-color: white;
	height: 0.6rem;
	width: 0.12rem;
	transform: rotate(135deg);
	border-radius: 25%;
}
.line2 {
	background-color: white;
	height: 0.6rem;
	width: 0.12rem;
	transform: rotate(-130deg);
	border-radius: 25%;
}
.regionmenu {
	position: fixed;
	background-color: rgba(52, 71, 83, 1);
	width: 7.5rem;
	line-height: 2rem;
	padding: 0 0 0 20px;
	border-radius: 5%;
	font-size: 1rem;
	margin-top: 2.8rem;
}

.regionmenu div:hover {
	font-size: 2rem;
}

.countrycontainer {
	display: grid;
	grid-template-columns: repeat(auto-fill, 250px);
	justify-content: center;
	column-gap: 20px;
	row-gap: 20px;
}
.countrylist {
	margin: 0 auto;
}

.countrylist {
	list-style-type: none;
}
.country {
	font-size: 10px;
}

.country img {
	margin-bottom: -2.5%;
	width: 100%;
	height: 15em;
	object-fit: cover;
}
.country h2 {
	font-size: 2em;
}
.country p {
	font-size: 1.6em;
	line-height: 1.5;
	font-family: Arial, Helvetica, sans-serif;
}
.textctn {
	margin-top: 10px;
}
.textback {
	background-color: rgba(52, 71, 83, 1);
	padding: 3em 2em;
	box-sizing: border-box;
	/* margin-left: 10%; */
}
.list {
	top: 11rem;
	left: 0.5rem;
	right: 0.5rem;
	margin: 10px;
	position: fixed;
	background-color: rgb(71, 103, 122);
	max-height: 15rem;
	overflow-y: auto;
	border-radius: 5px;
	padding: 0.5rem 1.5rem;
	max-width: 26rem;
}
.listmenu {
	width: 90%;
	background: rgb(71, 103, 122);
	margin: 1rem 0;
	list-style-type: none;
}
.listmenu div {
	color: aliceblue;
	font-size: 1.3rem;
}
.listmenu img {
	height: 1.5rem;
	width: 2rem;
	margin-right: 15px;
}
.backbtn {
	margin: 5% 0% 0% 10%;
	width: 3rem;
	padding: 15px;
	background-color: rgb(30, 81, 105);
	color: aliceblue;
	text-align: center;
	border-radius: 5%;
	cursor: pointer;
}
.singlecountry {
	display: flex;
	align-items: center;
	flex-direction: column;
	justify-content: center;
	margin-top: 5%;
	line-height: 2;
}
.singlecountry img {
	width: 20rem;
}
.singlecountry h2 {
	font-size: 3vh;
}
.singlecountry p {
	font-size: 2vh;
}
@media only screen and (min-width: 1024px) {
	.search {
		width: 430px;
	}
}
</style>
