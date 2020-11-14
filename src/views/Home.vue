<template>
  <div>
    <h3>Temperatura de las ultimas 10 personas que han pasado por la estación</h3>
    <line-chart></line-chart>
  </div>
</template>

<script>
import LineChart from '@/components/LineChart'

export default {
  components: {
    LineChart
  }
}
</script>