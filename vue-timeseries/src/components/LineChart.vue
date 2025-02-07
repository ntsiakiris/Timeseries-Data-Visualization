<script setup>
import { defineProps, ref, watch } from 'vue';
import { Chart, registerables } from 'chart.js';
import { Line } from 'vue-chart-3';

// Register Chart.js components
Chart.register(...registerables);

// Receive data from props
const props = defineProps({
  chartData: Array
});

// Reactive chart configuration
const chartConfig = ref({
  labels: props.chartData.map(item => item.name), // Use "name" as labels
  datasets: [
    {
      label: 'Value Over Time',
      data: props.chartData.map(item => item.value), // Use "value" as data points
      borderColor: 'rgba(75, 192, 192, 1)',
      backgroundColor: 'rgba(75, 192, 192, 0.2)',
      borderWidth: 2,
      fill: true,
      tension: 0.4 // Smooth line
    }
  ]
});

// Watch for changes (e.g., if user edits table)
watch(() => props.chartData, (newData) => {
  chartConfig.value.labels = newData.map(item => item.name);
  chartConfig.value.datasets[0].data = newData.map(item => item.value);
}, { deep: true });

</script>

<template>
  <div>
    <Line :data="chartConfig" :options="{ responsive: true, plugins: { title: { display: true, text: 'Trend Over Time' } } }" />
  </div>
</template>