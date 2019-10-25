<template>
  <div id="app">
    <ScrollText text="Sometimes the best way to solve your own problems... Is to help someone else."/>
  </div>
</template>

<script>
import ScrollText from './components/ScrollText.vue'

export default {
  name: 'app',
  components: {
    ScrollText
  }
}
</script>

<style>
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
html, body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px;
  margin-left: 10px;
  width: 300px;
  border: 1px solid black;
}
</style>
