<script>
  import { Bar, mixins } from 'vue-chartjs'
  const { reactiveProp } = mixins
  import axios from 'axios'
  // import Datepicker from 'vuejs-datepicker'

  export default {
    mixins: [reactiveProp, mixins.reactiveData],
    data() {
      return {
        chartData: '',
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
        }
      }
    },
    extends: Bar,
    mounted() {
      // this.render()
      this.fillData()
      this.watch()
      // this.$forceUpdate()
      this.renderChart(this.chartData, this.options)
      // this.update()
    },
    created() {
      // this.fillData()
    },
    watch(){
          this.$data._chart.update();
    },
    methods: {
      reload() {
      this.chartData.$forceUpdate()
      this.fillData()
      },
      fillData() {
          var tiemposURL = `https://apirestp2ace2.herokuapp.com/dato?fecha=`
      if(this.$fechaElegida == null){
        tiemposURL += `11/15/2020`
        axios.get(tiemposURL)
          .then(response => {
            // JSON responses are automatically parsed.
            console.log(response)
            const responseData = response.data
            // console.log(responseData[0].tiempo)
            // console.log(response.data[0].temperatura)
            // this.value = this.$refs.datepicker.pickedValue
            // console.log(this.value)
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
          })
          .catch(e => {
            this.errors.push(e)
          })
          console.log("url chart.vue: " + tiemposURL)
        } else {
          tiemposURL += this.$fechaElegida
          axios.get(tiemposURL)
          .then(response => {
            // JSON responses are automatically parsed.
            console.log(response)
            const responseData = response.data
            // console.log(responseData[0].tiempo)
            // console.log(response.data[0].temperatura)
            // this.value = this.$refs.datepicker.pickedValue
            // console.log(this.value)
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
          })
          .catch(e => {
            this.errors.push(e)
          })
          console.log("url chart.vue: " + tiemposURL)
        }
        
      }
    }
  }
</script>