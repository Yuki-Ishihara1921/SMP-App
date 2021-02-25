<template>
  <page-main />
</template>

<script>
import PageMain from './components/PageMain'

export default {
  name: 'App',
  components: {
    PageMain
  }
}
</script>

<style>
body {
  background: floralwhite !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
