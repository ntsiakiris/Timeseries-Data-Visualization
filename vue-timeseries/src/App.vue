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

  // Filter the data based on the start and end date
  const filteredData = jsonData.filter((item) => {
    const itemDate = new Date(item.DateTime);
    const start = new Date(startDate);
    const end = new Date(endDate);

    return itemDate >= start && itemDate <= end;
  });

  const datachecker = (filterData) => {
    if (filteredData.length === 0) {
      alert("No data available for the selected date range.");
    }
  };

  datachecker(filteredData);

  // Update table and chart data
  tableData.value = filteredData;
  chartData.value = filteredData;
};
</script>

<template>
  <DateRangePicker @filterData="filterData" class="filter-container" />
  <div class="app-container">
    <TableComponent :tableData="tableData" />
    <Chart :chartData="chartData" />
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

@media screen and (max-width: 1100px) {
  .app-container {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>
