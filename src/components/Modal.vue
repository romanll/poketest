<template>
    <b-modal id="modal">
        <div id="pokemon-data">
            <b-row cols="3">
                <b-col><img :src="modaldata.image"></b-col>
                <b-col>
                    <b-row cols="1">
                        <b-col class="modal-text">{{modaldata.id}}</b-col>
                        <b-col class="modal-name">{{modaldata.name}}</b-col>
                        <b-col>
                            <span :class="modaldata.type">{{modaldata.type}}</span>
                        </b-col>
                    </b-row>
                </b-col>
                <b-col>
                    <b-row cols="2">
                        <b-col class="modal-label">Height:</b-col>
                        <b-col class="modal-text">{{modaldata.height}}</b-col>
                    </b-row>
                    <b-row cols="2">
                        <b-col class="modal-label">Weight:</b-col>
                        <b-col class="modal-text">{{modaldata.weight}}</b-col>
                    </b-row>
                </b-col>
            </b-row>
            <b-row>
                <b-col>
                    Description
                </b-col>
            </b-row>
        </div>
        <!-- list and chart view -->
        <div id="list"></div>
        <div id="chart"></div>
    </b-modal>
</template>

<style scoped>
.modal-name {
    font-weight: 600;
    text-transform: capitalize;
    font-size: 1em;
}
.modal-label {
    font-weight: 600;
    font-size: .8em;
}
.modal-text {
    font-size: .8em;
}
</style>

<script>
export default {
    name: 'Modal',
    props: {
        modaldata: Object
    }
}
</script>