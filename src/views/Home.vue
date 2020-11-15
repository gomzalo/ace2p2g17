<template>


  <div>
    <h4> Ultima temperatura: <p id="ultimaTemp"></p> </h4>
    <h5>Temperatura de las ultimas 10 personas que han pasado por la estación</h5>
    <line-chart></line-chart>
    
  <table id="tableTemp">
      <thead class="thead-dark">
                    <tr>
                        <th>#</th>
                        <th>Temperatura [°]</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="temperatura_ in temperaturas" :key="temperatura_._id">
                        <td>{{temperatura_._id}}</td>
                        <td>{{temperatura_.temperatura}}</td>
                    </tr>
                </tbody>
  </table>
  </div>
</template>

<script>

import LineChart from '@/components/LineChart'
import axios from 'axios'
export default {
  components: {
     LineChart
  },
  el: "#tableTemp",
  data () {
    return {
      temperaturas: []
    }       
  },
  mounted() {
    this.getUltimas10Temp()
    this.getUltimaTemp()
  },
  methods: {
    getUltimas10Temp(){
      axios
      .get(`https://apirestp2ace2.herokuapp.com/tentemp`)
        .then(response => {
          // JSON responses are automatically parsed.
          
          this.temperaturas = response.data
          
          // console.log(response.data[0].temperatura)
        
        })
        .catch(e => {
          this.errors.push(e)
        })
    },
    getUltimaTemp(){
      axios
      .get(`https://apirestp2ace2.herokuapp.com/temperatura`)
        .then(response => {
          // JSON responses are automatically parsed.
          
          const temp  = response.data
          document.getElementById("ultimaTemp").innerHTML = temp[0].temperatura
          console.log(temp[0].temperatura)
        
        })
        .catch(e => {
          this.errors.push(e)
        })
    }
  }

}
</script>