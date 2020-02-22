<template>
  <div id=results>
      <search-page-buttons :total="searchResults.totalResults"></search-page-buttons>
      <ul>
          <li v-on:click="sendFilm(search.imdbID)" v-for="(search, index) in searchResults.Search" :key="index">{{search.Title}} | {{search.Year}}</li>
      </ul>
  </div>
</template>

<script>
import {eventBus} from '../main.js'
import SearchButtons from './SearchPageButtons.vue'
export default {
    name: 'search-results',
    props: ['searchResults'],
    data(){
        return{
            selectedFilm: null
        }
    },
    methods: {
        sendFilm: function(searchID){
            eventBus.$emit('send-film', searchID)
        }
    },
    components: {
        'search-page-buttons': SearchButtons
    }
}
</script>

<style>

ul{
    list-style: none;
}
li{    
    border-style:groove;
    padding: 10px;
    transition-duration: 500ms;
    transition-property: background, font-size;
}
li:hover{
    background: gray;
    font-size: 1.2em;
}
</style>