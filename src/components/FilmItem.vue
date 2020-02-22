<template>
  <div id=filmBlock>
      <div id=title>
          <h1>{{film.Title}}</h1>
                <img v-bind:src="film.Poster"/>
          </div>
          <button>Add To WatchList</button>
          <h3>{{film.Year}}</h3>
          <div id=ratings>
            <div id=rating v-for="(rating, index) in film.Ratings" :key="index">
                <h3 >{{rating.Source}}</h3>
                <p>{{rating.Value}}</p>
            </div>
          </div>
            
      <p>Box Office: {{film.BoxOffice}}</p>
  </div>
</template>

<script>
export default {
    name: 'film-item',
    props: ['film']
}
</script>

<style scoped>
#title{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
#filmBlock{
    display: flex;
    display: inline-block;
    border-style: dashed;
    width: 40vw;
}
#ratings{
}
img{
    height: 200px;
}
</style>