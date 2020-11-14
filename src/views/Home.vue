<template>
  <div>
    <h3>Temperatura de las personas que han pasado por la estación</h3>
    <line-chart></line-chart>
  </div>
</template>

<script>
import LineChart from '@/components/LineChart'
console.log("userlist")
export default {
  components: {
    LineChart
  }
}
</script>