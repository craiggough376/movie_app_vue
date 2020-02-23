<template>
  <div>
      <button v-on:click="deleteFilm()">Delete</button>
      <input type="checkbox" value="seen" v-model="seen">
      <label for="checkbox">Seen</label>
      <h3>{{film.Title}}</h3>
  </div>
</template>

<script>
import {eventBus} from '../main.js'
export default {
    name: 'favourites-list-item',
    props: ['film'],
    data(){
        return{
            seen: false
        }
    },
    methods:{
        deleteFilm(){
            eventBus.$emit('delete-film', this.$props.film)
        }
    }
}
</script>

<style>

</style>