<template>
    <b-row class="list-attribute">
        <b-col cols="3">
            <span class="attribute-value">{{stat.stat.name}}</span>
        </b-col>
        <b-col cols="9">
            <div class="bar-container">
                <div class="bar-background"></div>
                <div class="bar-gradient" :style="barWidth(stat.base_stat)"></div>
                <span class="bar-value" :style="statPosition(stat.base_stat)">{{stat.base_stat}}</span>
            </div>
        </b-col>
    </b-row>
</template>

<script>
export default {
    name: 'StatBar',
    props: {
        stat: Object
    },
    methods: {
        barWidth(value) {
            if (value > 100) value = 100
            return {'width': value.toString() + '%'}
        },
        statPosition(value) {
            if (value > 100) value = 100
            return {'left': 'calc(' + value + '% - 50px)'}
        }
    }
}
</script>

<style scoped>
.list-attribute {
    height: 33px;
    margin-bottom: 10px;
}
.attribute-value {
    color: #aaa;
    font-size: 1em;
    margin-top: 3px;
    display: block;
    text-transform: capitalize;
}
.bar-container {
    position: relative;
}
.bar-background {
    background-color: #ddd;
    height: 15px;
    border-radius: 4px;
    position: absolute;
    width: 100%;
    top: 10px;
}
.bar-gradient {
    position: absolute;
    top: 10px;
    background: rgb(25, 119, 182);
    background: linear-gradient(90deg, rgb(25, 119, 182) 0%, rgb(0, 174, 255) 100%);
    height: 15px;
    border-radius: 4px 0 0 4px;
    width: 50%;
}
.bar-value {
    font-weight: bold;
    font-size: .9em;
    border-radius: 4px;
    padding: 5px;
    text-align: center;
    box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
    position: absolute;
    background-color: #fff;
    width: 50px;
    top: 0;
}
</style>