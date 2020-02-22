<template>
  <div>
      <h1>Watch List</h1>
      <div v-for="(film, index) in favourites" :key="index">
          <favourites-list-item :film="film"></favourites-list-item>
          </div>
  </div>
</template>

<script>
import FavouritesListItem from './FavouritesListItem.vue'
export default {
    name: 'favourites-list',
    props: ['favourites'],
    data(){
        return{
        }
    },
    components: {
        'favourites-list-item': FavouritesListItem
    }
}
</script>

<style>

</style>