<template>
  <div ref="chart"/>
</template>

<script>
import axios from 'axios'
import {GoogleCharts} from 'google-charts'

const apiUrl = 'http://api.captainweb.net/stars'
const params = ['always', 'angulars', 'top']

export default {
  props: {
    ...params.reduce((previous, param) => ({
      ...previous,
      [param]: {type: Boolean}
    }), {})
  },
  computed: {
    apiUrl () {
      const queryString = params
        .filter(param => this[param])
        .map(param => param + '=1')
        .join('&')
      return apiUrl + (queryString ? '?' + queryString : '')
    }
  },
  methods: {
    drawChart () {
      axios.get(this.apiUrl).then(({data}) => {
        const dataTable = GoogleCharts.api.visualization.arrayToDataTable(data)
        const chart = new GoogleCharts.api.visualization.LineChart(this.$refs.chart)
        const options = {
          height: 600,
          hAxis: {title: 'Day'},
          vAxis: {title: 'Stars'}
        }
        chart.draw(dataTable, options)
        window.addEventListener('resize', () => {
          chart.draw(dataTable, options)
        }, false)
      })
    }
  },
  mounted () {
    GoogleCharts.load(this.drawChart, ['line'])
  }
}
</script>
