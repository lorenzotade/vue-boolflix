<template>
  <section>
    <div class="tvCard mb-3">
      <ul>
        <li><span>Titolo:</span> {{ show.name }}</li>
        <li><span>Titolo originale:</span> {{ show.original_name }}</li>
        <li><span>Lingua originale:</span> <CountryFlag :country="this.filterLang" size='small'/></li>
        <li><span>Voto:</span> {{ show.vote_average }}</li>
      </ul>
    </div>
  </section>
</template>

<script>
import CountryFlag from 'vue-country-flag'

export default {
  name: 'TvCard',
  components: {
    CountryFlag
  },
  props: {
    show: Object
  },
  data() {
    return {
      showObj: this.show
    }
  },
  computed: {
    filterLang() {
      if (this.showObj.original_language === 'en') {
        return 'gb'
      } else if (this.showObj.original_language === 'ja') {
        return 'jp'
      } else if (this.showObj.original_language === 'zh') {
        return 'cn'
      } else if (this.showObj.original_language === 'hi') {
        return 'in'
      } else if (this.showObj.original_language === 'he') {
        return 'il'
      } else if (this.showObj.original_language === 'cs') {
        return 'cz'
      } else if (this.showObj.original_language === 'ar') {
        return 'sa'
      } else if (this.showObj.original_language === 'ko') {
        return 'kr'
      }
      return this.showObj.original_language
    }
  }
}
</script>

<style lang="scss" scoped>
  .tvCard {
    background-color: bisque;
    flex-basis: calc(100% / 5);
    width: 250px;
    min-height: 150px;
    
    span {
      font-weight: 600;
    }
  }
</style>