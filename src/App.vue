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
.modal-open .modal-backdrop {
  opacity: 0.75;
  background: rgb(197,242,181);
  background: linear-gradient(90deg, rgba(197,242,181,1) 0%, rgba(110,204,200,1) 100%);
}

/*pagination*/
.pagination .page-item.active .page-link {
  background: rgb(197,242,181);
  background: linear-gradient(90deg, rgba(197,242,181,1) 0%, rgba(110,204,200,1) 100%);
  border: 1px solid rgba(0,0,0,0.1);
  border-color: none;
}
.page-item .page-link {
  color: #000;
  font-size: .9em;
}
</style>
