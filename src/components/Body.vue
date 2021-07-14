<template>
    <div>
        <b-container>
            <div v-show="noResults" id="no-results">
                <b-row>
                    <b-col cols="6" offset="3">
                        <b-alert show variant="warning">There are no results for your search</b-alert>
                    </b-col>
                </b-row>
            </div>
            <div v-show="!noResults" id="cards">
                <b-row cols="6">
                    <b-col
                        v-for="(pokemon,index) in pokemons"
                        :key="index"
                    >
                        <Card
                            :pokemon="pokemon"
                            :setModalData="setModalData"
                            :setResults="setResults"
                        />
                    </b-col>
                </b-row>
            </div>
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
    computed: {
        noResults() {
            return !this.hasResults
        }
    },
    data() {
        return {
            modalData: {},
            hasResults: true
        }
    },
    methods: {
        setModalData(data) {
            this.modalData = data
        },
        setResults(results) {
            this.hasResults = results
        }
    }
}
</script>

<style scoped>
#cards .col {
    margin-bottom: 15px;
}
.modal-dialog {
    max-width: 450px;
}
.alert.alert-warning {
    font-size: .7em;
    font-weight: bold;
}
</style>