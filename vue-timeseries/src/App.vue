<script setup>
import { ref } from "vue";
import TableComponent from "./components/DataTable.vue";
import Chart from "./components/Chart.vue";
import DateRangePicker from "./components/DateRangePicker.vue";
import jsonData from "./timeseries.json";

// Pass the data from the json file through the parent component
const tableData = ref(jsonData);
const chartData = ref(jsonData);

// Filter the table and chart data based on the selected date range
const filterData = ({ startDate, endDate }) => {
  if (!startDate || !endDate) return;

  // Get the start and end date
  const start = new Date(startDate);
  start.setHours(0, 0, 0, 0);

  const end = new Date(endDate);
  end.setHours(23, 59, 59, 999);

  // Filter the data
  const filteredData = jsonData.filter((item) => {
    const itemDate = new Date(item.DateTime);
    return itemDate >= start && itemDate <= end;
  });

  // Check if any data was found
  if (filteredData.length === 0) {
    alert("No data available for the selected date range.");
  }

  // Update table and chart data
  tableData.value = filteredData;
  chartData.value = filteredData;
};
</script>

<template>
  <div>
  <DateRangePicker @filterData="filterData" class="filter-container" />
  <div class="app-container">
    <TableComponent :tableData="tableData" />
    <Chart :chartData="chartData" />
  </div>
</div>
</template>

<style scoped>
.filter-container {
  display: flex;
  justify-content: center;
  align-content: flex-start;
  margin: 20px 0px;
}
.app-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  overflow: visible;
}

@media screen and (max-width: 1200px) {
  .app-container {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>
