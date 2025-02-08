<script setup>
import { defineProps, computed } from "vue";
import { Line } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement,
} from "chart.js"; // Import Chart.js components

// Register chart.js components
ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
  PointElement
);

const props = defineProps({
  chartData: Array,
});

//chart data and options
const chartOptions = {
  responsive: true,
  plugins: {
    title: {
      display: true,
      text:
        "Price Trends over Time, (click to the buttons below to select your timeseries)",
    },
  },
};

// Use computed to reactively generate the chart dataset
const chartDataset = computed(() => {
  return {
    labels: props.chartData.map((item) => item.DateTime),
    datasets: [
      {
        label: "DE Price (€)",
        data: props.chartData.map((item) => item.ENTSOE_DE_DAM_Price),
        borderColor: "rgba(255, 99, 132, 1)",
        tension: 0.4,
      },
      {
        label: "GR Price (€)",
        data: props.chartData.map((item) => item.ENTSOE_GR_DAM_Price),
        borderColor: "rgba(54, 162, 235, 1)",
        tension: 0.4,
      },
      {
        label: "FR Price (€)",
        data: props.chartData.map((item) => item.ENTSOE_FR_DAM_Price),
        borderColor: "rgba(75, 192, 192, 1)",
        tension: 0.4,
      },
    ],
  };
});
</script>

<template>
  <div class="Line-container">
    <Line :data="chartDataset" :options="chartOptions" />
  </div>
</template>

<style scoped>
.Line-container {
  position: sticky;
  width: 60%;
  top: 10px;
  max-height: 80vh;
}

@media screen and (max-width: 1100px) {
  .Line-container {
    width: 100%;
    min-height: 550px;
  }
}
</style>
