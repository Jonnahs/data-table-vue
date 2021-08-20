<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <img alt="Vue logo" src="./assets/md.svg">
  <DataTable/>
</template>

<script>
import DataTable from './components/DataTable.vue'

export default {
  name: 'App',
  components: {
    DataTable
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
  margin-top: 60px;
}
#app img {
  width: 100px;
  margin-right: 20px;
}
</style>