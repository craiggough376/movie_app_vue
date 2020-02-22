<template>
<div id=all>
  <div id=filmBlock :style="{backgroundImage: `url(${film.Poster}})`}">
          <div id=innerText>
            <div id=title>
                <h1>{{film.Title}}</h1>
                        <!-- <img v-bind:src="film.Poster"/> -->
                        </div>
                    <button v-on:click="addToFaves()">Add To WatchList</button>
                    <h3>{{film.Year}}</h3>
                <div id=ratings>
                    <div id=rating v-for="(rating, index) in film.Ratings" :key="index">
                        <h3 >{{rating.Source}}</h3>
                        <p>{{rating.Value}}</p>
                    </div>
                </div>
            <p>Box Office: {{film.BoxOffice}}</p>
            <p>{{film.Awards}}</p>
            <p>{{film.Plot}}</p>
         </div>
      </div>
      </div> 
</template>

<script>
import {eventBus} from '../main.js'
export default {
    name: 'film-item',
    props: ['film'],
    methods: {
        addToFaves: function(){
            eventBus.$emit('send-fave', this.$props.film)
        }
    }
}
</script>

<style scoped>
#title{
    display: flex;
    justify-content:flex-start;
    /* align-items: center; */
}
#all{
/* border-style: solid; */
}
#filmBlock{
    /* display: flex;
    display: inline-block; */
    /* border-style: dashed; */
    width: 45vw;
    padding: 2.5vh;
    transition-duration: 1s;
    overflow-wrap: break-word;
    transition-property: background, background-size;
    background: no-repeat center; 

}
#innerText{
    transition-duration: 1s;
    transition-property: opacity;
    width: 35vw;
    height: 80vh;
    opacity: 0;
}
#innerText:hover{
    opacity: 1 !important;
}

#filmBlock:hover{
    background: no-repeat right top;
    background-size: 125px;
    /* display: flex;
    justify-content: center */
}
#ratings{
    display: flex;
    align-items: center
}
img{
    height: 200px;
}

</style>