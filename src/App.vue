<template>
  <div id="app">
    <Title msg="Order your command"/>
    <DataGetter/>
  </div>
</template>

<script>
import Title from './components/Title.vue'
import DataGetter from './components/DataGetter'

export default {
  name: 'app',
  components: {
    DataGetter,
    Title
  }
}
</script>

<style lang="scss">
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
