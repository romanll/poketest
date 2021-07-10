<template>
  <div id="app">
    <Header/>
    <Body
      :pokemons="pokemons"
    />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Body from './components/Body.vue'

export default {
  name: 'App',
  components: {
    Header,
    Body
  },
  data() {
    return {
      pokemons: [],
      page:1
    }
  },
  methods: {
    //
  },
  mounted() {
    fetch('https://pokeapi.co/api/v2/pokemon?limit=18&offset=0',{
      method: 'get'
    }).then((response)=> {
      return response.json();
    }).then((data) => {
      this.pokemons = data.results;
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  background: url('./assets/pokepattern.jpg');
}
</style>
