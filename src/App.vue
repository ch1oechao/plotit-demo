<template>
  <div id="app">
    <brand></brand>
    <panel></panel>
  </div>
</template>

<script>
import Brand from './components/brand'
import Panel from './components/Panel'

export default {
  components: {
    Brand,
    Panel
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  font-family: Source Sans Pro, Helvetica, sans-serif;
}

#app {
  width: 100%;
  height: 100%;
  text-align: center;
  color: #FFF;
  background-color: #303841;
  overflow: scroll;
}
</style>
