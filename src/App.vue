<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    
    <vue-search @newitem="''" @itemselected="''" :source-field="'name'" :api-source="'https://jsonplaceholder.typicode.com/users'"></vue-search>

  </div>
</template>

<script>
import VueSearch from './components/VueSearch.vue'

export default {
  name: 'app',
  components: {
    VueSearch
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 600px;
  margin: auto;
}
 * { box-sizing: border-box; margin: 0; padding: 0; }
    body { margin: 1rem; }
</style>
