<template>
  <div id="app">
    <search-bar></search-bar>
    <router-view/>
  </div>   
</template>


<script>
import searchBar from '@/components/SearchBar'

export default {
  name: 'App',
  components:{
    searchBar
  }
}
</script>

