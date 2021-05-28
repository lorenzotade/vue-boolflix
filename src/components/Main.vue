<template>
  <main class="container">

    <h3 v-if="this.filmList.length != 0">Film</h3>
    <div class="page-console">
      <button 
        v-if="this.filmList.length != 0" 
        :disabled="filmData.page === 1" 
        @click="$emit('pagPrec', apiPage-1)"
        >
          Prec.
        </button>
      <button 
        v-if="this.filmList.length != 0" 
        :disabled="filmData.page === filmData.total_pages" 
        @click="$emit('pagSucc', apiPage+1)"
      >
        Succ.
      </button>
    </div>
    
    <div class="card-container">
      <h2 v-if="this.filmList.length === 0">Nessun film trovato</h2>
      <Card 
        :card="card"
        v-for="card in filmList" :key="card.id"
      />
    </div>

    <h3 v-if="this.tvList.length != 0">Serie TV</h3>
    <div class="page-console">
      <button 
        v-if="this.tvList.length != 0" 
        :disabled="tvData.page === 1" 
        @click="$emit('pagPrec', apiPage-1)"
      >
        Prec.
      </button>
      <button 
        v-if="this.tvList.length != 0" 
        :disabled="tvData.page === tvData.total_pages"
        @click="$emit('pagSucc', apiPage+1)"
      >
        Succ.
      </button>
    </div>
    
    <div class="card-container">
      <h2 v-if="this.tvList.length === 0">Nessuna serie TV trovata</h2>
      <Card 
        :card="card"
        v-for="card in tvList" :key="card.id"
      />
    </div>
    
  </main>
</template>

<script>
import Card from '/src/components/Card.vue'

export default {
  name: 'Main',
  components: {
    Card,
  },
  props: {
    filmList: Array,
    filmData: Object,
    tvList: Array,
    tvData: Object,
    apiPage: Number
  },
}
</script>

<style lang="scss" scoped>
  main {
    background-color: #434343;
    padding-top: 20px;
    min-height: calc(100vh - 80px);
    h2 {
      color: #FFF;
    }
    h3 {
      font-size: 30px;
      color: #FFF;
      text-transform: uppercase;
      margin: 20px 0;
    }
    .card-container {
      display: flex;
      justify-content: space-between;
      overflow-x: scroll;
    }
    .page-console {
      text-align: center;
      button {
        padding: 10px 20px;
        background-color: #FFF;
        border: 1px solid #000;
        margin: 0 10px;
        margin-bottom: 20px;
        text-transform: uppercase;
        font-weight: 600;
        &:active {
          background-color: #F00;
        }
      }
    }
  }
  
  
</style>
