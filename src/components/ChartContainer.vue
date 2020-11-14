<template>
  <div class="container">
      
    <line-chart
      v-if="loadedline"
      :chartdata="chartdataline"
      :options="optionsline"/>

  </div>
</template>

<script>
import LineChart from './LineChart.vue'

export default {
  name: 'LineChartContainer',
  components: { LineChart },
  data: () => ({
    loaded: false,
    chartdata: null
  }),
  async mounted () {
    this.loadedline = false
    try {
      const { userlist } = await fetch('https://apirestp2ace2.herokuapp.com/temperatura')

    console.log("userlist")
    console.log(userlist)
    var c = 0
    // for(userlist)
      this.chartdataline = userlist
      this.loadedline = true
    } catch (e) {
      console.error(e)
    }
  }
}
</script>