<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    
    <vue-search 
    :img-photo="'https://people.planningcenteronline.com/static/no_photo_thumbnail_gray.png?g=68x68%23'" 
    @newitem="newitem()" 
    @itemselected="itemselected($event)" 
    :source-field="'name'" 
    :show-new-botton="true"
    :search-by-field="false"
    :api-source="'https://jsonplaceholder.typicode.com/users'"
    ></vue-search>

  </div>
</template>

<script>
import VueSearch from './components/VueSearch.vue'

export default {
  name: 'app',
  components: {
    VueSearch
  },
  methods:{
    itemselected(ev){
      console.log('ok',ev)
    },
    newitem(){
      console.log('newitem')
    },
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
