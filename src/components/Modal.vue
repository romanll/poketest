<template>
    <b-modal 
        id="modal" 
        hide-footer 
        hide-header 
        body-class="m-body"
        no-close-on-backdrop
        centered
        ref="pokemon-modal"
    >
        <button type="button" aria-label="Close" class="close close-modal" @click="closeModal">×</button>
        <div id="pokemon-data">
        <b-row cols="3">
            <b-col><img :src="modaldata.image" class="modal-image" /></b-col>
            <b-col>
            <b-row cols="1">
                <b-col class="modal-text">{{ modaldata.id }}</b-col>
                <b-col class="modal-name">{{ modaldata.name }}</b-col>
                <b-col>
                <span :class="[modaldata.type, 'modal-type']">{{
                    modaldata.type
                }}</span>
                </b-col>
            </b-row>
            </b-col>
            <b-col>
            <b-row cols="2">
                <b-col class="modal-label">Height:</b-col>
                <b-col class="modal-text">{{ modaldata.height }}</b-col>
            </b-row>
            <b-row cols="2">
                <b-col class="modal-label">Weight:</b-col>
                <b-col class="modal-text">{{ modaldata.weight }}</b-col>
            </b-row>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
            {{ modaldata.description }}
            </b-col>
        </b-row>
        </div>
        <!-- list and chart view -->
        <b-row cols="3" class="statistics-header">
        <b-col class="statistics-title-bar">
            <div></div>
        </b-col>
        <b-col class="statistics-title">Statistics</b-col>
        <b-col class="statistics-title-bar">
            <div></div>
        </b-col>
        </b-row>
        <div id="list" v-show="!chartView">
        <StatBar
            v-for="(stat, index) in modaldata.stats"
            :key="index"
            :stat="stat"
        ></StatBar>
        </div>
        <div id="chart" v-show="chartView">
            <canvas id="graph"></canvas>
        </div>
        <b-row>
            <b-col cols="3" offset="4">
                <span class="modal-text">Chart View</span>
            </b-col>
            <b-col cols="3">
            <b-form-checkbox v-model="chartView" name="check-button" switch>
            </b-form-checkbox>
            </b-col>
        </b-row>
    </b-modal>
</template>

<style>
@import 'https://unpkg.com/bootstrap@4.5.2/dist/css/bootstrap.min.css';
#pokemon-data {
  margin-bottom: 10px;
}
.modal-dialog.modal-md {
  max-width: 450px;
}
.modal-body.m-body {
  padding: 2rem;
}
.modal-image {
  float: right;
}
.modal-name {
  font-weight: 600;
  text-transform: capitalize;
  font-size: 1em;
}
.modal-label {
  font-weight: 600;
  font-size: 0.8em;
  text-align: right;
}
.modal-text {
  font-size: 0.85em;
}
.close.close-modal {
    position: absolute;
    top: -10px;
    right: -10px;
    border: 1px solid #888;
    background-color: #fff;
    z-index: 1;
    opacity: 1;
    padding: 0 4px 3px 4px;
    padding-top: -19px;
    color: #333;
    line-height: 18px;
    padding-bottom: 3px;
    border-radius: 2px;
}
.modal-body .close.close-modal:focus,.modal-body .close.close-modal:hover {
    opacity: 1;
}
.modal-type {
  font-size: 0.7em;
  padding: 2px 7px;
  font-weight: 600;
  border-radius: 2px;
  text-transform: uppercase;
  background-color: #ddd;
}
.statistics-header {
  margin-bottom: 15px;
}
.statistics-title-bar {
  align-self: center;
}
.statistics-title-bar div {
  border-top: 2px solid #ddd;
}
.statistics-title {
  text-align: center;
  text-transform: uppercase;
  font-size: 0.9em;
}
.custom-control-input:checked ~ .custom-control-label::before {
    border-color: #5de600;
    background-color: #50df00;
}
.custom-switch .custom-control-label::before {
    border-color: #bbb;
    background-color: #ccc;
}
.custom-switch .custom-control-label::after {
    background-color: #fff;
}

.grass {
  background-color: rgb(125, 172, 16);
  color: #fff;
}
.fire {
  background-color: rgb(231, 112, 0);
  color: #fff;
}
.water {
  background-color: rgb(0, 158, 231);
  color: #fff;
}
.bug {
  background-color: rgb(14, 124, 20);
  color: #fff;
}
.normal {
  background-color: rgb(236, 201, 0);
  color: #333;
}
</style>

<script>
import StatBar from "./StatBar.vue";
import Chart from "chart.js/auto"
export default {
    name: "Modal",
    props: {
        modaldata: Object,
    },
    data(){
        return {
            chartView: false,
            chart: Chart
        }
    },
    components: {
        StatBar,
    },
    watch: {
        chartView: {
            handler() {
                if(this.chartView) {
                    this.chart = document.getElementById('graph').getContext('2d');
                    let chartData = {};
                    let max = 100;
                    this.modaldata.stats.forEach((e) => {
                        let name = e.stat.name.charAt(0).toUpperCase() + e.stat.name.slice(1)
                        chartData[name] = e.base_stat
                        if(e.base_stat > max) max = Math.ceil(e.base_stat/20)*20
                    })
                    const config = {
                        type: "radar",
                        data: {
                            labels: Object.keys(chartData),
                            datasets: [{
                                data: Object.values(chartData),
                                fill: true,
                                backgroundColor: "rgba(97, 176, 255, 0.3)",
                                borderColor: "rgba(97, 160, 255, 1)",
                                pointBackgroundColor: "rgba(97, 160, 255, 1)",
                                pointBorderColor: "#fff",
                                pointHoverBackgroundColor: "#fff",
                                pointHoverBorderColor: "rgba(97, 160, 255, 1)",
                            }],
                        },
                        options: {
                            responsive: true,
                            plugins: {
                                title: {
                                    display: false,
                                },
                                legend: {
                                    display:false
                                }
                            },
                            scale: {
                                min:0,
                                max:max,
                            }
                        },
                    };
                    this.chart = new Chart(this.chart, config);
                }
                else {
                    this.chart.destroy()
                }
            },
            immediate: false
        }
    },
    methods: {
        closeModal() {
            this.$refs['pokemon-modal'].hide()
            this.chart.destroy()
        }
    }
};
</script>