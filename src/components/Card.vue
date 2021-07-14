<template>
    <b-card
        :title="name"
        :img-src="image"
        :img-alt="name"
        img-top
        align="center"
        @click="showData"
    >
        <b-card-text>
            {{pokemonId}}
        </b-card-text>
    </b-card>
</template>

<style scoped>
.card:hover {
    cursor: pointer;
}
.card-title {
    text-transform: capitalize;
    font-size: 1em;
    font-weight: 600;
}
.card-text {
    font-size: .75em;
    color: #777;
}

</style>

<script>
export default {
    name: 'Card',
    props:{
        pokemon: Object,
        setModalData: Function,
        setResults: Function
    },
    data() {
        return {
            pokemonData : Object,
            pokemonSpecies: Object
        }
    },
    methods: {
        showData() {
            //set data to modal
            let mainType = [...this.pokemonData.types].shift();
            let modalData = {
                id : this.pokemonId,
                name: this.pokemonData.name,
                height: (this.pokemonData.height / 10).toString() + 'm',
                weight: (this.pokemonData.weight / 10).toString() + 'kg',
                image: this.image,
                type: mainType.type.name,
                stats: this.pokemonData.stats,
                description:this.pokemonSpecies.flavor_text_entries[0].flavor_text
            }
            this.setModalData(modalData)
            this.$bvModal.show('modal');
        },
        getPokemonData() {
            this.setResults(true)
            fetch(this.pokemon.url,{
                method: 'get'
            }).then((response)=> {
                if (response.ok) return response.json();
                return response.ok
            }).then((data) => {
                if(data) {
                    this.pokemonData = data;
                    fetch(data.species.url, {
                        method:'get'
                    }).then((response) => {
                        return response.json();
                    }).then((data) => {
                        this.pokemonSpecies = data
                    })
                }
                else {
                    //to hide cards and show alert
                    this.setResults(false)
                }
            }).catch(err => {
                console.log(err)
            })
        }
    },
    computed: {
        pokemonId() {
            if(this.pokemonData.id !== undefined) {
                let id = this.pokemonData.id.toString();
                while(id.length < 3) {
                    id = "0" + id;
                }
                return "#" + id;
            }
            return '#';
        },
        image() {
            if(this.pokemonData.sprites !== undefined){
                return this.pokemonData.sprites.front_default
            }
            return require('../assets/spinner.gif');
        },
        name() {
            return this.pokemon.name.length ? this.pokemon.name : this.pokemonData.name
        }
    },
    watch: {
        pokemon: {
            handler() {
                this.getPokemonData()
            },
            immediate: true
        }
    }
}
</script>