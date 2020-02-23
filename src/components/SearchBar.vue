<template>
    <div>
    <input type="text" placeholder="Search for a movie..." v-model="searchString" style="width: 300px; height: 17px;">
    <button v-on:click="sendSearchString()">Search</button>
    </div>
</template>

<script>
import {eventBus} from '../main.js'
export default {
    name: 'search-bar',
    data(){
        return{
            searchString: ''
        }
    },
    methods:{
        sendSearchString: function(){
            eventBus.$emit('search-string', this.searchString)
        }
    }
}
</script>

<style>

</style>