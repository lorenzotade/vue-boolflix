<template>
  <main class="container">

    <h2 v-if="this.filmList.length === 0 && this.tvList.length === 0">Nessun risultato trovato</h2>

    <h3 v-if="this.filmList.length != 0">Film</h3>
    <div class="card-container">
      <FilmCard 
        :film="film"
        v-for="film in filmList" :key="film.id"
      />
    </div>
  
    <h3 v-if="this.tvList.length != 0">Show</h3>
    <div class="card-container">
      <TvCard 
        :show="show"
        v-for="show in tvList" :key="show.id"
      />
    </div>
    
  </main>
</template>

<script>
import FilmCard from '/src/components/FilmCard.vue'
import TvCard from '/src/components/TvCard.vue'

export default {
  name: 'Main',
  components: {
    FilmCard,
    TvCard
  },
  props: {
    filmList: Array,
    tvList: Array
  },
}
</script>

<style lang="scss" scoped>
  main {
    background-color: #000;
    padding-top: 20px;
    min-height: calc(100vh - 80px);
    h2 {
      color: #FFF;
    }
    h3 {
      font-size: 30px;
      color: #FFF;
      text-transform: uppercase;
      margin: 30px 0;
    }
    .card-container {
      display: flex;
      justify-content: space-between;
      flex-wrap: wrap;
    }
  }
  
  
</style>
