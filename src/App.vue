<template>
  <div id="app">
    <h1>GitHub Stars</h1>
    <chart/>
    <br>
    <chart always/>
    <hr>
    <chart angulars/>
    <br>
    <chart always angulars/>
    <hr>
    <chart top3/>
    <br>
    <chart always top3/>
    <hr>
    <chart angulars top3/>
    <br>
    <chart always angulars top3/>
  </div>
</template>

<script>
import Chart from './components/Chart'

export default {
  components: {
    Chart
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  line-height: 18px;
  background: #f0f0f0;
  font-family: Arial, sans-serif;
}

h1 {
  position: relative;
  margin: 20px 0 40px;
  padding: 0 0 20px;
  font-size: 22px;
  text-align: center;
  text-transform: uppercase;
}

h1:after {
  position: absolute;
  left: 50%;
  bottom: 0;
  width: 70px;
  height: 1px;
  margin-left: -35px;
  background: #262626;
  content: '';
}
</style>
