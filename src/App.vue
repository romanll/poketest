<template>
  <div id="app">
    <Header
      :searchLocal="searchLocal"
      :searchApi="searchApi"
    />
    <Body
      :pokemons="pokemons"
    />
    <div class="overflow-auto" v-show="showPagination">
      <div class="mt-3">
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          per-page="perPage"
          align="center"
        ></b-pagination>
      </div>
    </div>
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
      pokemonsFromApi: [],
      currentPage:1,
      rows:100,
      perPage: 18,
      offset: 0,
      search: ''
    }
  },
  computed: {
    pokemons: {
      get() {
        if(this.pokemonsFromApi.length == 1) return this.pokemonsFromApi
        return this.pokemonsFromApi.filter(pokemon => {
          return pokemon.name.toLowerCase().includes(this.search.toLowerCase())
        })
      },
      set(resultFromSearchApi) {
        this.pokemonsFromApi = resultFromSearchApi
      }
    },
    showPagination() {
      return this.pokemons.length == this.perPage
    }
  },
  watch: {
    currentPage: {
      handler() {
        this.getPokemons()
      },
      immediate: true
    }
  },
  methods: {
    searchLocal(keyword) {
      this.search = keyword
    },
    //get pokemons from API
    getPokemons() {
      if(this.currentPage > 1) {
        this.offset = this.perPage * (this.currentPage - 1)
      }
      else {
        this.offset = 0
      }
      fetch(`https://pokeapi.co/api/v2/pokemon?limit=${this.perPage}&offset=${this.offset}`,{
        method: 'get'
      }).then((response)=> {
        return response.json();
      }).then((data) => {
        this.pokemonsFromApi = data.results;
        this.rows = data.count;
      })
    },
    searchApi(keyword) {
      //simulate a result from getPokemons function
      let link = 'https://pokeapi.co/api/v2/pokemon/' + keyword
      this.pokemons = [{
        name: '',
        url: link
      }]
    }
  },
}
</script>

<style>

#app {
  /*font-family: Avenir, Helvetica, Arial, sans-serif;*/
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  background: url('./assets/pokepattern.jpg');
}

/*pagination*/
.pagination .page-item.active .page-link {
  background: rgb(0,255,152);
  background: linear-gradient(90deg, rgba(0,255,152,1) 0%, rgba(1,236,215,1) 98%);
  border: 1px solid rgba(0,0,0,0.1);
  border-color: none;
}
.page-item .page-link {
  color: #000;
  font-size: .9em;
}
</style>
