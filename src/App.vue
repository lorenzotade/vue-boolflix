<template>
  <div id="app">
    <Header
      @searchQuery="this.searching"
    />
    <Main 
      :filmList="filmList"
    />
  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header.vue'
import Main from '@/components/Main.vue'

export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  data() {
    return {
      apiURL: 'https://api.themoviedb.org/3/search/movie',
      apiKey: '5ec3e7079b3cb189d8fa0d92bd66a1c9',
      apiLang: 'it-IT',
      apiQuery: '',
      filmList: []
    }
  },
  methods: {
    searching(apiQuery) {
      this.apiQuery = apiQuery;
      axios.get(this.apiURL, {
        params: {
          api_key: this.apiKey,
          query: this.apiQuery,
          language: this.apiLang
        }
      })
      .then(res => {
        this.filmList = res.data.results;
      })
      .catch(err => {
        console.log(err);
      });
      this.apiQuery = '';
    }
  },
}
</script>

<style lang="scss">
  @import '@/assets/style/general.scss';
</style>
