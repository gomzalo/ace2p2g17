<template>
  <div>
    <h3>Tiempo que tardan las personas en el lugar</h3>
    <datepicker placeholder="Elige una fecha" 
    :value='fecha' :language="es" format='MM-dd-yyyy' @selected="CallDateFunction"></datepicker>
    
    <bar-chart></bar-chart>
    <table id="tableTemp">
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
import axios from 'axios'
import { es } from 'vuejs-datepicker/dist/locale'

export default {
  components: {
    BarChart, Datepicker
  },
   el: "#tableTemp",
  data () {
    return {
      tiempos: [],
      format: "d MMMM yyyy",
      es: es,
      fecha : ''
    }       
  },
  mounted: function() {
    axios
      .get(`https://apirestp2ace2.herokuapp.com/dato?fecha=11/15/2020`)
        .then(response => {
          // JSON responses are automatically parsed.
          
          this.tiempos = response.data
          
          // console.log(response.data[0].temperatura)
        
        })
        .catch(e => {
          this.errors.push(e)
        })
  },
  methods: {
    CallDateFunction (value) {
      console.log("updating datepicker value");
      this.$fechaElegida = String(value).format('MM/DD/YY')
      console.log(this.$fechaElegida)
      // console.log(value)
      this.fecha = value;
      
    }
  }
}
</script>
