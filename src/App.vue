<template>
  <div id="app">
    <Header
      @searchQuery="searching"
    />

    <Main 
      :filmList="filmList"
      :filmData="filmData"
      :popMovies="popMovies"
      :tvList="tvList"
      :tvData="tvData"
      :popTv="popTv"
      :apiPage="apiPage"
      @pagPrec="pagPrec"
      @pagSucc="pagSucc"
    />
  </div>
</template>

<script>

import axios from 'axios'
import Header from './components/Header.vue'
import Main from './components/Main.vue'

export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  data() {
    return {
      apiMoviesURL: 'https://api.themoviedb.org/3/search/movie',
      apiTvURL: 'https://api.themoviedb.org/3/search/tv',
      apiKey: '5ec3e7079b3cb189d8fa0d92bd66a1c9',
      apiLang: 'it-IT',
      apiPage: 1,
      apiQuery: '',
      filmList: [],
      tvList: [],
      filmData: {},
      tvData: {},
      popMovies: [],
      popTv: []
    }
  },
  methods: {
    // chiamata API per prendere i dati film e serie + quelli relativi alle pagine;
    // si scatena al click del bottone di ricerca o al keyup enter emettendo la 
    // query di ricerca inserita dall'utente (String)
    searching(apiQuery) {
      this.apiQuery = apiQuery;
      this.apiPafe = 1;
      let request = {
        params: {
          api_key: this.apiKey,
          query: this.apiQuery,
          language: this.apiLang,
          page: this.apiPage
        }
      }
      if (apiQuery != '') {
        axios.all([
            axios.get(this.apiMoviesURL, request),
            axios.get(this.apiTvURL, request),
          ])
        .then(axios.spread((resMovies, resTv) => {
          this.filmData = resMovies.data;
          this.filmList = resMovies.data.results;
          this.tvData = resTv.data;
          this.tvList = resTv.data.results;
        }))
        .catch(err => {
          console.log(err);
        })
      }
      
    },
    // metodi scatenati al click dei bottoni prec. e succ. per ripetere la chiamata
    // API con numero di pagina aggiornato 
    pagPrec(apiPage) {
      this.apiPage = apiPage;
      this.searching(this.apiQuery);
    },
    pagSucc(apiPage) {
      this.apiPage = apiPage;
      this.searching(this.apiQuery);
    },
  },
  // al created dell'istanza effettua due chiamate API per recuperare film e serie tv
  // in tendenza in quella settimana
  created() {
    axios.all([
      axios.get("https://api.themoviedb.org/3/trending/movie/week?api_key=" + this.apiKey),
      axios.get("https://api.themoviedb.org/3/trending/tv/week?api_key=" + this.apiKey)
    ])
      .then(axios.spread((resPopMovies, resPopTv) => {
        this.popMovies = resPopMovies.data.results;
        this.popTv = resPopTv.data.results;
      }))
      .catch(err => {
        console.log(err);
      })
  }
}

</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>