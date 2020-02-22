<template>
  <div id="app">
    <h2>Movie Marker &#127909;</h2>
    <button v-on:click="showFavourites()">Show WatchList</button>
    <p>You have {{this.favouritesArray.length}} movies saved</p>
    <search-bar></search-bar>
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
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#main{
  display: flex;
  justify-content: center;

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
</style>
