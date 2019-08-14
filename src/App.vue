<template>
  <div id="app">
    <org-chart/>
  </div>
</template>

<script>
// import TodoList from './components/TodoList.vue'
import OrgChart from './components/OrgChart.vue'

export default {
  name: 'app',
  components: {
    // TodoList
    OrgChart
  }
  
  
}
</script>

<style>
@import './main.css';
</style>
