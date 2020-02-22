<template>
  <div id="app">
    <h2>My Movies</h2>
    <search-bar></search-bar>
    <search-results v-if="this.searchResults"  :searchResults="this.searchResults"></search-results>
  </div>
</template>

<script>
import {eventBus} from './main.js'
import SearchBar from './components/SearchBar.vue'
import SearchResults from './components/SearchResults.vue'
export default {
  name: 'App',
  components: {
    'search-bar': SearchBar,
    'search-results': SearchResults
  },
  data(){
    return{
      searchResults: null
    }
  },
  methods: {
    fetchMovies: function(searchString) {
      fetch(`http://www.omdbapi.com/?s=${searchString}&page=1&apikey=82fc1890`)
      .then(response => response.json())
      .then(searchResults => this.searchResults = searchResults)
    }
  },
  computed: {

  },
  mounted(){
    eventBus.$on('search-string', enteredString => {this.fetchMovies(enteredString)})
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
</style>
