<template>
<div id=buttons>
    <div v-for="page in this.totalPages" :key="page">
      <button v-on:click="sendPage(page)" v-if="page<=10">{{page}}</button>
    </div>
</div>
</template>

<script>
import {eventBus} from '../main.js'
export default {
    name: 'search-page-buttons',
    props: ['total'],
    computed:{
        totalPages: function(){
            return Math.round(this.$props.total/10)
        }
    },
    methods: {
        sendPage: function(page){
            eventBus.$emit('send-page', page)
        }
    }
}
</script>

<style>
#buttons{
    display: flex;
    justify-content: center;
}
</style>