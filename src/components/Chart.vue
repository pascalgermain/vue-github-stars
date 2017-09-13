<template>
  <div class="chart">
    <div ref="chart" />
    <label>
      <input type="checkbox" v-model="always"> Always
    </label>
  </div>
</template>

<script>
import {GoogleCharts} from 'google-charts'

export default {
  data () {
    return {
      stats: [],
      always: false,
      loaded: false
    }
  },
  computed: {
    apiUrl () {
      return 'http://api.captainweb.net/stars' + (this.always ? '?always=1' : '')
    }
  },
  watch: {
    always () {
      this.drawChart()
    }
  },
  methods: {
    getQueryParam (name) {
      name = name.replace(/[[\]]/g, '\\$&')
      const regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)')
      const results = regex.exec(window.location.href)
      if (!results) return null
      if (!results[2]) return ''
      return decodeURIComponent(results[2].replace(/\+/g, ' '))
    },
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
      if (!this.loaded) return
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
  created () {
    this.always = this.getQueryParam('always') !== null
  },
  mounted () {
    GoogleCharts.load(() => {
      this.loaded = true
      this.drawChart()
    }, ['line'])
  }
}
</script>

<style scoped>
.chart {
  position: relative;
}

label {
  position: absolute;
  right: 0;
  bottom: 0;
  padding: 30px;
  font-size: 16px;
  cursor: pointer;
}
</style>
