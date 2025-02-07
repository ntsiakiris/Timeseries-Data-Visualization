<script setup>
import { ref } from 'vue';
import TableComponent from './components/DataTable.vue';
import ChartComponent from './components/Chart.vue';
import jsonData from './timeseries.json'; // Adjust the path to your JSON file

// Table data, made reactive to allow updates from the table to reflect in the chart
const tableData = ref(jsonData);
</script>

<template>
  <div>
    <!-- Display Table and Chart -->
    <TableComponent :tableData="tableData" />
    <ChartComponent :chartData="tableData" />
  </div>
</template>

<style scoped>
/* Add custom styles */
</style>
