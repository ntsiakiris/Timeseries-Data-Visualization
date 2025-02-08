<script setup>
import { ref, defineEmits, watch } from "vue";

// Create an event emitter to communicate with the parent component
const emit = defineEmits(["filterData"]);

// Reactive values for the date range
const startDate = ref("");
const endDate = ref("");

let debounceTimeout;

// Handle the date range change and emit the filtered data (debouncing)
const applyFilter = () => {
  emit("filterData", { startDate: startDate.value, endDate: endDate.value });
};

// Watch for changes in startDate and endDate d
watch([startDate, endDate], () => {
  // Clear previous timeout to reset the debounce
  clearTimeout(debounceTimeout);

  // trigger the filter after 500ms
  debounceTimeout = setTimeout(() => {
    applyFilter();
  }, 500);
});
</script>

<template>
  <div class="date-picker-container">
    <label for="filter-container">Choose the date to filter your data:</label>
    <div class="filter-container">
      <label for="startDate">Start Date:</label>
      <input type="date" v-model="startDate" />
      <label for="endDate">End Date:</label>
      <input type="date" v-model="endDate" />
    </div>
  </div>
</template>

<style scoped>
.date-picker-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

.filter-container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px 0;
  width: 100%;
}

input {
  padding: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
  min-width: 150px;
}

label {
  padding: 5px;
}

input[type="date"] {
  padding: 5px 15px;
}

input:focus {
  border-color: #4caf50;
}
</style>
