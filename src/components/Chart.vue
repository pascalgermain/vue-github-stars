<template>
  <div ref="chart"/>
</template>

<script>
import axios from 'axios'
import {GoogleCharts} from 'google-charts'

export default {
  props: {
    always: {
      type: Boolean
    }
  },
  computed: {
    apiUrl () {
      return 'http://api.captainweb.net/stars' + (this.always ? '?always=1' : '')
    }
  },
  methods: {
    drawChart () {
      axios.get(this.apiUrl).then(response => {
        const data = GoogleCharts.api.visualization.arrayToDataTable(response.data)
        const chart = new GoogleCharts.api.visualization.LineChart(this.$refs.chart)
        const options = {
          height: 600,
          hAxis: {title: 'Day'},
          vAxis: {title: 'Stars'}
        }
        chart.draw(data, options)
        window.addEventListener('resize', () => {
          chart.draw(data, options)
        }, false)
      })
    }
  },
  mounted () {
    GoogleCharts.load(this.drawChart, ['line'])
  }
}
</script>
