<script>

import { Line, mixins } from 'vue-chartjs'
import axios from 'axios'

export default {
  mixins: [mixins.reactiveData],
  data() {
    return{
        chartData: '',
         options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: false
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [ {
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
  
  extends: Line,
  mounted () {
      this.renderChart(this.chartData)
    },
    created() {
      axios.get(`https://apirestp2ace2.herokuapp.com/temperatura`)
        .then(response => {
          // JSON responses are automatically parsed.
          
          const responseData = response.data
          console.log(responseData[0].temperatura)
          // console.log(response.data[0].temperatura)
        
          this.chartData  = 
          // responseData.temperatura
          {            

            // labels: responseData.map(item => item.id),
            labels: ["1",	"2",	"3",	"4",	"5",	"6",	"7", "8", "9", "10"],
            
            // data: responseData.map(item => item.id),

            datasets: [
              {
                label: 'Temperatura en grados',
                backgroundColor: '#f87979',
                data: responseData.map(item => item.temperatura)
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