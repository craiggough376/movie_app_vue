<template>
  <div>
      <p>Total Results: {{searchResults.totalResults}}</p>
      <ul>
          <li v-on:click="sendFilm(search.imdbID)" v-for="(search, index) in searchResults.Search" :key="index">{{search.Title}}</li>
      </ul>
  </div>
</template>

<script>
import {eventBus} from '../main.js'
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