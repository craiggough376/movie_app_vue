<template>
  <div id="app">
    <h2>My Movies</h2>
    <search-bar></search-bar>
      <div id=main>
        <search-results :class="!this.filmById ? 'results-full':'results-half'" v-if="this.searchResults" :searchResults="this.searchResults"></search-results>
        <film-item v-if="this.filmById" :film="this.filmById"></film-item>
      </div>
  </div>
</template>

<script>
import {eventBus} from './main.js'
import SearchBar from './components/SearchBar.vue'
import SearchResults from './components/SearchResults.vue'
import FilmItem from './components/FilmItem.vue'
export default {
  name: 'App',
  components: {
    'search-bar': SearchBar,
    'search-results': SearchResults,
    'film-item': FilmItem
  },
  data(){
    return{
      searchResults: null,
      filmById: null,
      searchString: null,
      pageNumber: 1
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
    }
  },
  computed: {

  },
  mounted(){
    eventBus.$on('search-string', enteredString => {this.searchString = enteredString, this.pageNumber = 1, this.fetchMovies(enteredString)})
    eventBus.$on('send-film', filmId => {this.fetchFilm(filmId)})
    eventBus.$on('send-page', (page) => {this.pageNumber = page, this.fetchMovies(this.searchString)})
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
.results-full{
  width: 90vw;
}
.results-half{
  width: 40vw;
  padding: 5vw;
}
</style>
