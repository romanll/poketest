<template>
    <b-card
        :title="pokemon.name"
        :img-src="image"
        :img-alt="pokemon.name"
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
        setModalData: Function
    },
    data() {
        return {
            pokemonData : Object
        }
    },
    methods: {
        showData() {
            //set data to modal
            let mainType = this.pokemonData.types.shift();
            let modalData = {
                id : this.pokemonId,
                name: this.pokemonData.name,
                height: (this.pokemonData.height / 10).toString() + 'm',
                weight: (this.pokemonData.weight / 10).toString() + 'kg',
                image: this.image,
                type: mainType.type.name
            }
            this.setModalData(modalData)
            //open a popup
            this.$bvModal.show('modal');
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
        }
    },
    mounted() {
        fetch(this.pokemon.url,{
            method: 'get'
            }).then((response)=> {
                return response.json();
            }).then((data) => {
                this.pokemonData = data;
        })
    }
}
</script>