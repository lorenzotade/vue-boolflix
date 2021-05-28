<template>
  <section>
    <div class="card">
      <img v-if="card.poster_path === null" src="#" :alt="card.title || card.name">
      <img v-if="card.poster_path != null" :src="`https://image.tmdb.org/t/p/w300${card.poster_path}`" :alt="card.name">
      <div class="description-container">
        <ul>
          <li><span>Titolo:</span> {{ card.title || card.name}}</li>
          <li><span>Titolo originale:</span> {{ card.original_title || card.original_name}}</li>
          <li>
            <span>Lingua originale:</span> 
            <CountryFlag :country="this.filterLang" size='small'/>
          </li>
          <li>
            <span>Voto: </span> 
            <i v-for="(i, index) in this.convertVote" :key="index+'star1'" class="fas fa-star"></i>
            <i v-for="(i, index) in (5 - this.convertVote)" :key="index+'star2'" class="far fa-star"></i>
          </li>
          <li v-if="card.overview != ''"><span>Overview:</span> {{ card.overview }}</li>
        </ul>
      </div>
      
    </div>
  </section>
  
</template>

<script>
import CountryFlag from 'vue-country-flag'
import axios from 'axios'

export default {
  name: 'Card',
  components: {
    CountryFlag
  },
  props: {
    card: Object
  },
  /* PER AXIOS CHE NON FUNZIONA */
  data() {
    return {
      movieURL: "https://api.themoviedb.org/3/movie/" + this.card.id + "/credits?api_key=5ec3e7079b3cb189d8fa0d92bd66a1c9&language=it-IT",
      showURL: "https://api.themoviedb.org/3/tv/" + this.card.id + "/credits?api_key=5ec3e7079b3cb189d8fa0d92bd66a1c9&language=it-IT",
      castMovie: [],
      castShow: []
    }
  },
  computed: {
    filterLang() {
      if (this.card.original_language === 'en') {
        return 'gb'
      } else if (this.card.original_language === 'ja') {
        return 'jp'
      } else if (this.card.original_language === 'zh') {
        return 'cn'
      } else if (this.card.original_language === 'hi') {
        return 'in'
      } else if (this.card.original_language === 'he') {
        return 'il'
      } else if (this.card.original_language === 'cs') {
        return 'cz'
      } else if (this.card.original_language === 'ar') {
        return 'sa'
      } else if (this.card.original_language === 'ko') {
        return 'kr'
      } else if (this.card.original_language === 'sv') {
        return 'se'
      }
      return this.card.original_language
    },
    convertVote() {
      return Math.ceil((this.card.vote_average * 5) / 10)
    }
  },
  /* NON FUNZIONA */
  mounted() {
    axios.all([
        axios.get(this.movieURL),
        axios.get(this.showURL)
      ])
    .then(axios.spread((creditMovie, creditShow) => {
      this.castMovie = creditMovie.cast;
      this.castShow = creditShow.cast;
    }))
    .catch(err => {
      console.log(err);
    })
    console.log('cast movie', this.castMovie);
    console.log('cast show', this.castShow);
  }
}
</script>

<style lang="scss" scoped>
  
</style>