<template>
  <b-modal id="modal" hide-footer hide-header body-class="m-body">
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
    <div id="list">
      <StatBar
        v-for="(stat, index) in modaldata.stats"
        :key="index"
        :stat="stat"
      ></StatBar>
    </div>
    <div id="chart">
        <canvas id="graph"></canvas>
    </div>
    <div>
        <b-form-checkbox v-model="checked" name="check-button" switch>
        Switch Checkbox <b>(Checked: {{ checked }})</b>
        </b-form-checkbox>
    </div>
  </b-modal>
</template>

<style>
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
.modal-type {
  font-size: 0.7em;
  padding: 2px 7px;
  font-weight: 600;
  border-radius: 2px;
  text-transform: uppercase;
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
          checked:false
      }
  },
  components: {
    StatBar,
  },
  updated() {
    let chartData = {};
    let max = 100;
    this.modaldata.stats.forEach((e) => {
        let name = e.stat.name.charAt(0).toUpperCase() + e.stat.name.slice(1)
        chartData[name] = e.base_stat
        if(e.base_stat >100) max = max = 120
    })
    let chart = document.getElementById('graph').getContext('2d');
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
    new Chart(chart, config);
  },
};
</script>