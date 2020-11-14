<script>
  import { Bar, mixins } from 'vue-chartjs'
  import axios from 'axios'

  export default {
    mixins: [mixins.reactiveData],
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
      this.renderChart(this.chartData, this.options)
    },
    created() {
      axios.get(`https://apirestp2ace2.herokuapp.com/dato?fecha=11/14/2020`)
        .then(response => {
          // JSON responses are automatically parsed.
          console.log(response)
          const responseData = response.data
          // console.log(responseData[0].tiempo)
          // console.log(response.data[0].temperatura)
        
          this.chartData  = 
          // responseData.temperatura
          {            

            // labels: responseData.map(item => item.id),
            labels: ["1",	"2",	"3",	"4",	"5",	"6",	"7", "8", "9", "10"],
            
            // data: responseData.map(item => item.id),

            datasets: [
              {
                label: 'Tiempo en minutos',
                backgroundColor: '#f87979',
                data: responseData.map(item => item.tiempo)
              }
            ]
          }
        })
        .catch(e => {
          this.errors.push(e)
        })
  }
  }
</script>