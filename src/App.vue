<template>
  <div id="app">
  <!-- <link href="https://fonts.googleapis.com/css?family=Anton|Teko&display=swap" rel="stylesheet">  -->
     <div id="header">
      <h1>Movie Marker &#127909;</h1>
      <search-bar></search-bar>
      <button v-on:click="showFavourites()">Show WatchList</button>
    </div>
    <p v-if="this.favouritesArray.length > 1">You have {{this.favouritesArray.length}} movies saved</p>
    <favourites-list :favourites="this.favouritesArray" v-if="this.favourites && !this.searchResults"></favourites-list>
      <div id=main v-if="this.searchResults">
        <search-results :class="!this.filmById ? 'results-full':'results-half'" :searchResults="this.searchResults"></search-results>
        <film-item class="filmItem" v-if="this.filmById" :film="this.filmById"></film-item>
      </div>
  </div>
</template>

<script>
import {eventBus} from './main.js'
import SearchBar from './components/SearchBar.vue'
import SearchResults from './components/SearchResults.vue'
import FilmItem from './components/FilmItem.vue'
import FavouritesList from './components/FavouritesList.vue'

export default {
  name: 'App',
  components: {
    'search-bar': SearchBar,
    'search-results': SearchResults,
    'film-item': FilmItem,
    'favourites-list': FavouritesList
  },
  data(){
    return{
      searchResults: null,
      filmById: null,
      searchString: null,
      pageNumber: 1,
      favourites: false,
      favouritesArray: []
    }
  },
  methods: {
    fetchMovies: function(searchString) {
      fetch(`http://www.omdbapi.com/?s=${searchString}&page=${this.pageNumber}&apikey=82fc1890`)
      .then(response => response.json())
      .then(searchResults => this.searchResults = searchResults)
    },
    fetchFilm: function(id){
      fetch(`http://www.omdbapi.com/?i=${id}&page=1&apikey=82fc1890`)
      .then(response => response.json())
      .then(filmById => this.filmById = filmById)
    },
    showFavourites(){
      this.favourites = true
      this.searchResults = false
    },
    deleteFilm(film){
      for(let i=0; i<this.favouritesArray.length; i++){
        if(this.favouritesArray[i].Title = film.Title){
          this.favouritesArray.splice(this.favouritesArray[i], 1)
        }
      }
    }
  },
  computed: {

  },
  mounted(){
    eventBus.$on('search-string', enteredString => {this.searchString = enteredString, this.pageNumber = 1, this.fetchMovies(enteredString)})
    eventBus.$on('send-film', filmId => {this.fetchFilm(filmId)})
    eventBus.$on('send-page', (page) => {this.pageNumber = page, this.fetchMovies(this.searchString)})
    eventBus.$on('send-fave', (film) => {this.favouritesArray.push(film)})
    eventBus.$on('delete-film', (film) => {this.deleteFilm(film)})
  }
}
</script>

<style>
#app {
  font-family: Impact, Charcoal, sans-serif;
  	

  font-family: 'Anton', ;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: lightblue;
}
#main{
  display: flex;
  justify-content: center;
}
#header{
  display: flex;
  align-items:baseline;
  justify-content: space-around;
  background-color:whitesmoke;

}
h1{
  font-size: 1.8em;
}
.filmItem{
  transition: width 2s;
  transition-timing-function: ease-in;
}
.results-full{
  width: 90vw;
}
.results-half{
  transition-duration: 1s;
  width: 45vw;
}
button {
  border-style:solid;
  border-radius: 3px;
  /* padding: 15px 32px; */
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  /* height: 10px; */


}
button:hover{
  background-color: grey;
}
</style>
