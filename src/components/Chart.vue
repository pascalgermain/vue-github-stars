<template>
  <div ref="chart"/>
</template>

<script>
import {GoogleCharts} from 'google-charts'

export default {
  props: {
    always: {
      type: Boolean
    }
  },
  data () {
    return {
      stats: []
    }
  },
  computed: {
    apiUrl () {
      return 'http://api.captainweb.net/stars' + (this.always ? '?always=1' : '')
    }
  },
  methods: {
    fetchStats (callback) {
      const xhr = new XMLHttpRequest()
      xhr.open('GET', this.apiUrl)
      xhr.onload = () => {
        this.stats = JSON.parse(xhr.responseText)
        if (typeof callback === 'function') callback()
      }
      xhr.send()
    },
    drawChart () {
      this.fetchStats(() => {
        const data = GoogleCharts.api.visualization.arrayToDataTable(this.stats)
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
