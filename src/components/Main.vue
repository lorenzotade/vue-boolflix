<template>
  <main class="container">

    <h2 v-if="this.filmList.length != 0">Film</h2>
    <div class="card-container">
      <FilmCard 
        :film="film"
        v-for="film in filmList" :key="film.id"
      />
    </div>
  
    <h2 v-if="this.tvList.length != 0">Show</h2>
    <div class="card-container">
      <TvCard 
        :show="show"
        v-for="show in tvList" :key="show.id"
      />
    </div>
    
  </main>
</template>

<script>
import FilmCard from '@/components/FilmCard.vue'
import TvCard from '@/components/TvCard.vue'

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
  .card-container {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
  }
</style>
