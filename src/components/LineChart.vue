<script>

import { Line, mixins } from 'vue-chartjs'
import axios from 'axios'

export default {
  mixins: [mixins.reactiveData],
  data() {
    return{
        temperaturaData: []
    }
  },
  
  extends: Line,
  mounted () {
      this.renderChart(this.temperaturaData)
    },
    created() {
      axios.get(`https://apirestp2ace2.herokuapp.com/temperatura/`)
        .then(response => {
          // JSON responses are automatically parsed.
          
          const responseData = response.data
          console.log(responseData[0].temperatura)
          // console.log(response.data[0].temperatura)
        
          this.temperaturaData  = 
          // {
              responseData.temperatura

            // labels: responseData.map(item => item.temperatura),
            
            // data: responseData.map(item => item.id)


            // datasets: [
            //   {
            //     label: 'Temperatura en grados',
            //     backgroundColor: '#f87979',
            //     data: responseData.map(item => item.id)
            //   }
            // ]
          // }
        })
        .catch(e => {
          this.errors.push(e)
        })
  }
}
</script>