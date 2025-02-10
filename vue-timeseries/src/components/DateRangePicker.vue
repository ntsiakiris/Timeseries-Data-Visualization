<script setup>
import { ref, defineEmits, watch } from "vue";

const emit = defineEmits(["filterData"]);

const startDate = ref("");
const endDate = ref("");

let debounceTimeout;

// Handle the date range change and emit the filtered data
const applyFilter = () => {
  emit("filterData", { startDate: startDate.value, endDate: endDate.value });
};

// Watch for changes in startDate and endDate date
watch([startDate, endDate], () => {
  // Clear previous timeout (debouncing)
  clearTimeout(debounceTimeout);

  // trigger the filter after 500ms
  debounceTimeout = setTimeout(() => {
    applyFilter();
  }, 500);
});
</script>

<template>
  <div class="date-picker-container">
    <label for="filter-container" class="title">Filter your data <span>📅</span></label>
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

.title {
  background: rgba(0, 162, 255, 0.2);
  color: #00a2ff;
  border-radius: 8px;
  border: 2px solid rgba(0, 162, 255, 0.5);
  box-shadow: 0 4px 10px rgba(0, 162, 255, 0.3);
  backdrop-filter: blur(10px);
  font-weight: bold;
}

@media screen and (max-width: 768px) {
  label {
    font-size: small;
    padding: 5px 0;
  }
  .date-picker-container {
  display: flex;
  justify-content: center;
  align-items: center;
 
}

}

</style>
