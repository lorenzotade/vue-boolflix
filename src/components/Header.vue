<template>
  <header class="mb-3">
    <div>LOGO</div>
    <div>
      <input 
        type="text" 
        placeholder="Cerca il film..." 
        v-model="apiQuery"
        @keyup.enter="sendQuery()"
      >
      <button @click="sendQuery()">CERCA</button>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      apiQuery: ''
    }
  },
  methods: {
    sendQuery() {
      this.$emit('searchQuery', this.apiQuery);
      this.apiQuery = '';
    }
  }
}
</script>

<style lang="scss" scoped>

</style>