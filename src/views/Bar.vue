<template>
  <div>
    <h3>Tiempo que tardan las personas en el lugar</h3>
    <datepicker placeholder="Elige una fecha" 
    :value='fecha' :language="es" format='MM-dd-yyyy' @selected="CallDateFunction"></datepicker>
    
    <bar-chart :chart-data="chartData"></bar-chart>
    <table id="tableTempT">
      <thead class="thead-dark">
                    <tr>
                        <th>#</th>
                        <th>Tiempo [min]</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="tiempo_ in tiempos" :key="tiempo_._id">
                        <td>{{tiempo_._id}}</td>
                        <td>{{tiempo_.tiempo}}</td>
                    </tr>
                </tbody>
  </table>
  </div>
</template>

<script>
import BarChart from '@/components/BarChart'
import Datepicker from 'vuejs-datepicker'
import { Bar, mixins } from 'vue-chartjs'
import axios from 'axios'
import { es } from 'vuejs-datepicker/dist/locale'

export default {
  mixins: [mixins.reactiveData],
  components: {
    BarChart,
    Datepicker
  },
  el: "#tableTempT",
  
  data () {
    return {
      tiempos: [],
      chartData: '',
      format: "MMMM d yyyy",
      es: es,
      fecha : '',
      options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false
        },
    }       
  },
  extends: Bar,
  mounted() {
    this.fillData()
    this.renderChart(this.chartData) 
    this.watch()
    // console.log(tiemposURL)
  },
  created(){
    this.renderChart(this.chartData) 
    this.fillData()
  },
  watch(){
    this.$data._chart.update();
  },
  methods: {
      reload() {
      this.$forceUpdate()
      },
      CallDateFunction (value) {
      console.log("updating datepicker value");
// -- Formato a fecha
      this.$fechaElegida = this.formatDate(value)
      console.log(this.$fechaElegida)
      // console.log(value)
      this.fecha = value;
      // fecha
      this.fillData()
      this.renderChart(this.chartData)
      this.watch()
      this.$forceUpdate()
    },
    formatDate(date) {
    var d = new Date(date),
        month = '' + (d.getMonth() + 1),
        day = '' + d.getDate(),
        year = d.getFullYear();

    if (month.length < 2) 
        month = '0' + month;
    if (day.length < 2) 
        day = '0' + day;

    return [month, day, year].join('/');
    },
    fillData(){
      var tiemposURL = `https://apirestp2ace2.herokuapp.com/dato?fecha=`
       
    if(this.$fechaElegida == null){
      tiemposURL += `11/15/2020`
    axios
      .get(tiemposURL)
        .then(response => {
          // JSON responses are automatically parsed.
          
          this.tiempos = response.data
          const responseData = response.data
          console.log(responseData.length)
          if(responseData.length < 1){
            alert('No hay datos en la fecha seleccionada')
          }
          this.chartData  = 
          // responseData.temperatura
          {            

            // labels: responseData.map(item => item.id),
            labels: ["1",	"2",	"3",	"4",	"5",	"6",	"7", "8", "9", "10"],
            
            // data: responseData.map(item => item.id),

            datasets: [
              {
                label: 'Tiempo en minutos',
                backgroundColor: '#2a466e',
                data: responseData.map(item => item.tiempo)
              }
            ]
          }
          // console.log(response.data[0].temperatura)
        
        })
        .catch(e => {
          this.errors.push(e)
        })
    } else {
      tiemposURL += this.$fechaElegida
      axios
      .get(tiemposURL)
        .then(response => {
          // JSON responses are automatically parsed.
          
          this.tiempos = response.data
          const responseData = response.data
          console.log(responseData.length)
          if(responseData.length < 1){
            alert('No hay datos en la fecha seleccionada')
          }
          this.chartData  = 
          // responseData.temperatura
          {            

            // labels: responseData.map(item => item.id),
            labels: ["1",	"2",	"3",	"4",	"5",	"6",	"7", "8", "9", "10"],
            
            // data: responseData.map(item => item.id),

            datasets: [
              {
                label: 'Tiempo en minutos',
                backgroundColor: '#2a466e',
                data: responseData.map(item => item.tiempo)
              }
            ]
          }
          // console.log(response.data[0].temperatura)
        
        })
        .catch(e => {
          this.errors.push(e)
        })
      }
      console.log("url from bar.vue" + tiemposURL)
    }
  }
}
</script>
