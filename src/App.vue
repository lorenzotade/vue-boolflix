<template>
  <div id="app">
    <Header
      @searchQuery="searching"
    />

    <Main 
      :filmList="filmList"
      :filmData="filmData"
      :tvList="tvList"
      :tvData="tvData"
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
      tvData: {}
    }
  },
  methods: {
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
            axios.get(this.apiTvURL, request)
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
    pagPrec(apiPage) {
      this.apiPage = apiPage;
      this.searching(this.apiQuery);
    },
    pagSucc(apiPage) {
      this.apiPage = apiPage;
      this.searching(this.apiQuery);
    },
  },
}
</script>

<style lang="scss">
  @import './assets/style/general.scss';
</style>
