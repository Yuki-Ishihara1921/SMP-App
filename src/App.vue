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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

body {
  background: floralwhite !important;
}

.btn {
  box-shadow: 0px 1px 0px 2px #555;
}
</style>
