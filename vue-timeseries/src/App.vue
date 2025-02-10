<script setup>
import { ref } from "vue";
import TableComponent from "./components/DataTable.vue";
import Chart from "./components/Chart.vue";
import DateRangePicker from "./components/DateRangePicker.vue";
import jsonData from "./timeseries.json";

// Make tableData reactive
const tableData = ref(jsonData);
const chartData = ref(jsonData);

// Function to filter table and chart data based on date range
const filterData = ({ startDate, endDate }) => {
  if (!startDate || !endDate) return;

  const start = new Date(startDate);
  start.setHours(0, 0, 0, 0);

  const end = new Date(endDate);
  end.setHours(23, 59, 59, 999);

  const filteredData = jsonData.filter((item) => {
    const itemDate = new Date(item.DateTime);
    return itemDate >= start && itemDate <= end;
  });

  if (filteredData.length === 0) {
    alert("No data available for the selected date range.");
  }

  tableData.value = filteredData;
  chartData.value = filteredData;
};
</script>

<template>
  <div> 
    <h2>Timeseries Data Visualization</h2>
    <DateRangePicker @filterData="filterData" class="filter-container" />
    <div class="app-container">
      <TableComponent v-model="tableData" />
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

h2 {
  text-align: center;
  margin-top:10px;
  font-size: xx-large;
  font-weight: 600;
  font-family: 'Roboto', sans-serif;
  color: #2de4f1;
  text-transform: uppercase;
  text-shadow: 1px 1px 0px #1d74f7,
               1px 2px 0px #1d74f7,
               1px 3px 0px #1d74f7,
               1px 4px 0px #1d74f7,
               1px 5px 0px #1d74f7,
               1px 6px 0px #1d74f7,
               1px 10px 5px rgba(16, 16, 16, 0.5),
               1px 15px 10px rgba(16, 16, 16, 0.4),
               1px 20px 30px rgba(16, 16, 16, 0.3),
               1px 25px 50px rgba(16, 16, 16, 0.2);
  
  
}

@media screen and (max-width: 1200px) {
  .app-container {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>

