<template>
  <section>
    <!-- quando con il mouse entro per la prima volta su una carta, chiamo la funzione per recuperare il cast -->
    <div @mouseenter.once="getCrew()" class="card">

      <img v-if="card.poster_path === null" src="#" :alt="card.title || card.name">
      <img v-if="card.poster_path != null" :src="`https://image.tmdb.org/t/p/w300${card.poster_path}`" :alt="card.name">
      <!-- sezione description -->
      <div class="description-container">

        <ul>
          <li>
            <span>Titolo:</span> {{ card.title || card.name}}
          </li> <!-- /titolo -->
          <li>
            <span>Titolo originale:</span> {{ card.original_title || card.original_name}}
          </li> <!-- /titolo originale -->
          <li>
            <span>Lingua originale:</span> 
            <CountryFlag :country="this.filterLang" /> 
          </li> <!-- lingua -->
          <li>
            <span>Voto: </span> 
            <i 
              v-for="(i, index) in this.convertVote" :key="index+'star1'" 
              class="fas fa-star">
            </i>
            <i 
              v-for="(i, index) in (5 - this.convertVote)" :key="index+'star2'" 
              class="far fa-star">
            </i>
          </li> <!-- /voto -->
          <li>
            <span>Cast: </span>
            <span 
              class="cast" 
              v-for="(actor, index) in cast.slice(0,5)" :key="index">{{ actor.name }}, 
            </span>
          </li> <!-- /cast -->
          <li 
            v-if="card.overview != ''">
            <span>Overview:</span> {{ card.overview }}
          </li> <!-- /overview -->
        </ul> <!-- /lista description -->

      </div> <!-- /sezione description -->
      
    </div> <!-- /card -->
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
    card: Object,
    type: String
  },
  data() {
    return {
      apiURL: {
        movie: "https://api.themoviedb.org/3/movie/" + this.card.id + "/credits?&api_key=5ec3e7079b3cb189d8fa0d92bd66a1c9",
        tv: "https://api.themoviedb.org/3/tv/" + this.card.id + "/credits?&api_key=5ec3e7079b3cb189d8fa0d92bd66a1c9"
      },
      cast: []
    }
  },
  computed: {
     // proprietà computata per sistemare la visualizzazione della bandiera le iniziali delle cui lingue
     // differiscono dal codice ISO-2 dello Stato 
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
  methods: {
    // chiamata API per recuperare il cast di un determinato film o serie tv
    getCrew() {
      axios.get(this.apiURL[this.type])
        .then(res => {
          this.cast = res.data.cast;
        })
        .catch(err => {
          console.log(err);
        })
    }
  }
}

</script>

<style lang="scss" scoped>
</style>