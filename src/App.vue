<template>
  <div id="app">
    <monitor-view></monitor-view>
  </div>
</template>

<script>
import MonitorView from './views/MonitorView'
export default {
  name: 'App',
  components: {
    MonitorView
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
