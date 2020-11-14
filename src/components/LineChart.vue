<script>
import { Line } from 'vue-chartjs'
import axios from 'axios'
export default {
  data() {
    return{
      dailyLabels: [],
      dailyData: []
    }
  },
  extends: Line,
  mounted () {
    this.renderChart(
          {
        labels: ['Monday', 'Tuesday', 'Wednesday', 'Thursday ', 'Friday', 'Saturday', 'Sunday'],
        datasets: [
          {
            label: 'Daily Students',
            backgroundColor: '#f87979',
            data: [12, 20, 1, 50, 10, 40, 18]
          }
        ]
      })
    },
    created() {
      axios.get(`https://apirestp2ace2.herokuapp.com/temperatura/`)
        .then(response => {
          // JSON responses are automatically parsed.
          this.console.log(response)
          this.dailyData = response.data.days
        })
        .catch(e => {
          this.errors.push(e)
        }
      )
  }
  
}
</script>