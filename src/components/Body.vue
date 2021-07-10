<template>
    <div>
        <b-container>
            <div id="cards">
                <b-row cols="6">
                    <b-col 
                        v-for="(pokemon,index) in pokemons"
                        :key="index"
                    >
                        <Card
                            :pokemon="pokemon"
                            :setModalData="setModalData"
                        />
                    </b-col>
                </b-row>
            </div>
            <div id="pagination"></div>
            <!-- modal -->
            <Modal
                :modaldata="modalData"
            />
        </b-container>
    </div>
</template>

<script>
import Card from './Card.vue'
import Modal from './Modal.vue'
export default {
    components: {
        Card,
        Modal
    },
    name: 'Body',
    props: {
        pokemons: [],
    },
    data() {
        return {
            modalData: Object
        }
    },
    computed: {
    },
    methods: {
        setModalData(data) {
            this.modalData = data
        }
    }
}
</script>

<style scoped>
#cards .col {
    margin-bottom: 15px;
}
</style>