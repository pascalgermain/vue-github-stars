<template>
  <div ref="chart"/>
</template>

<script>
import axios from 'axios'
import {GoogleCharts} from 'google-charts'

const apiUrl = 'http://api.captainweb.net/stars'

export default {
  props: {
    always: {
      type: Boolean
    }
  },
  computed: {
    apiUrl () {
      return this.always ? apiUrl + '?always=1' : apiUrl
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
