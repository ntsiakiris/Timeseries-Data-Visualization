<script setup>
import { ref } from 'vue';
import TableComponent from './components/DataTable.vue';
import Chart from './components/Chart.vue';
import jsonData from './timeseries.json';

// Table data, made reactive to allow updates from the table to reflect in the chart
const tableData = ref(jsonData);
</script>

<template>
  <div class="app-container">
    <!-- Display Table and Chart -->
    <TableComponent :tableData="tableData" />
    <Chart :chartData="tableData" />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  overflow: visible;
}

@media screen and (max-width: 1100px) {
  .app-container {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  
}
</style>
