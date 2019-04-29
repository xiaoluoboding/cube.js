<template>
  <ve-line-chart :data="chartData" :settings="chartSettings" />
</template>

<script>
import dayjs from 'dayjs'

export default {
  name: 'VeChart',
  props: {
    dimensions: Array,
    measures: Array,
    resultSet: {
      type: Object,
      required: true,
    }
  },
  created () {
    const seriesNames = this.resultSet.seriesNames()
    const chartPivot = this.resultSet.chartPivot()
    const dimensions = chartPivot.length > 0 ? Object.keys(chartPivot[0]) : []
    let dimensionsData = []
    const measuresData = []
    seriesNames.forEach((e) => {
      const data = chartPivot.map(v => v[e.key]);
      measuresData.push({ name: e.key, data });
      dimensionsData = chartPivot.map(v => dayjs(v.x).format('YYYY-MM-DD'))
    });
    const chartData = {
      dimensions: {
        name: 'DateTime',
        data: dimensionsData
      },
      measures: measuresData
    }
    this.chartData = chartData
    console.log(this.chartData)
    this.chartSettings = {
      smooth: true
    }
  }
}
</script>

<style>

</style>
