<template>
  <v-card class="ma-4 main_card">
    <v-list-group :value="true"> 
      <template v-slot:activator>
          <v-icon size="30" >mdi-chart-box</v-icon>
          <v-list-item-title><h1>Global Results</h1></v-list-item-title>
      </template>
      <div class="ma-2 main_div" v-if="has_data_to_show">
        <div class="ma-2">
          <pie-chart :chartData="getPieData()" ></pie-chart>
        </div>
        <div class="ma-2 tab_div">
          <v-simple-table class="ma-1">
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-center">
                    Metric
                  </th>
                  <th class="text-center">
                    Value
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="item in Object.keys(getGlobalMetrics())"
                  :key="item"
                >
                  <td>{{ item }}</td>
                  <td>{{ getGlobalMetrics()[item]}}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </div>
      </div>
      <div v-else>
        <p class="ma-4">No results to show. Please upload a results file first.</p>
      </div>
      <!--div class="ma-2 main_div">
        <div class="ma-2">
          <LineChart :chartData="line1" ></LineChart>
        </div>
        <div class="ma-2">
          <LineChart :chartData="line2" ></LineChart>
        </div>
      </div-->
    </v-list-group>
    
  </v-card>
</template>

<script>

//import LineChart from "./charts/LineChart.vue";
import PieChart from "./charts/PieChart.vue";

export default {
  components: {PieChart},
  name: 'MainResultsCard',
  props: {
    results: {
      type: Object,
      default: () => {
        return {
          'per_component_consumption': {
              'undefined': 1,
          },
          'total_energy:': 0
        }
      }
    }
  },
  data() {
    return {
      pie: {
         labels: [
            'CPU',
            'Screen',
            'WI-FI',
            'GPU',
            'Bluetooth',
            'NFC'
        ],
        datasets: [{
            data: [300, 50, 100,500,103,4],
            backgroundColor: [
            'rgb(255, 99, 132)',
            'rgb(54, 162, 235)',
            'rgb(255, 205, 86)',
            'rgb(255, 100, 86)',
            'rgb(255, 205, 6)',
            'rgb(255, 100, 186)'
            ],
            hoverOffset: 4
        }]
      },
      line1: {
         labels: [
            1,2,3,4,5
        ],
        datasets: [{
            label: 'CPU frequency',
            data: [300, 310, 315,330,320],
            backgroundColor: [
            'rgb(54, 162, 235)',
            ],
            hoverOffset: 4
        }]
      },
    }
  },
  computed: {
    has_data_to_show(){
      return Object.keys(this.results).length != 0
    }
  },
  methods: {
    getPieData() {
      if (Object.keys(this.results).length == 0){
        return {}
      }
      const labels = Object.keys(this.results.per_component_consumption)
      const datasets = [
        {
          data: labels.map(x => this.results.per_component_consumption[x]),
          backgroundColor: this.getColors()
        }
      ]
      return {
        labels: labels,
        datasets: datasets
      }
    },
    getGlobalMetrics(){
      
      if (Object.keys(this.results).length == 0){
        return {}
      }
      // eslint-disable-next-line no-unused-vars
      const { per_component_consumption, stats,  ...resultsWithoutComponentConsumption } = this.results;
      return resultsWithoutComponentConsumption
    },
    getColor(idx){
      const colors = [
            'rgb(255, 100, 200)',
            'rgb(54, 162, 235)',
            'rgb(255, 205, 86)',
            'rgb(255, 100, 86)',
            'rgb(255, 5, 6)',
            'rgb(5, 205, 66)',
            'rgb(255, 25, 16)',
            'rgb(54, 162, 25)',
            'rgb(25, 205, 250)',
            'rgb(205, 100, 186)']
      return colors[idx % colors.length]
    },
    getColors(){
      return [
            'rgb(255, 100, 200)',
            'rgb(54, 162, 235)',
            'rgb(255, 205, 86)',
            'rgb(255, 100, 86)',
            'rgb(255, 5, 6)',
            'rgb(5, 205, 66)',
            'rgb(255, 25, 16)',
            'rgb(54, 162, 25)',
            'rgb(25, 205, 250)',
            'rgb(205, 100, 186)']
    }
  }
}
</script>


<style>


.main_card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.main_div {
  display: flex;
  flex-direction: row;
  align-items: center;
  width:100%;
}

.tab_div {
  display: flex;
  flex-direction: column;
}


h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

</style>
