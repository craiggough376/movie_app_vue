<template>
  <div>
      <ul>
          <li v-on:click="sendFilm(search.imdbID)" v-for="(search, index) in searchResults.Search" :key="index">{{search.Title}}</li>
      </ul>
       <p>Total Results: {{searchResults.totalResults}}</p>
       <search-page-buttons v-if="searchResults.totalResults" :total="searchResults.totalResults"></search-page-buttons>
  </div>
</template>

<script>
import {eventBus} from '../main.js'
import SearchButtons from './SearchPageButtons.vue'
export default {
    name: 'search-results',
    props: ['searchResults', 'currentPage'],
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
    border-style: double
}

</style>