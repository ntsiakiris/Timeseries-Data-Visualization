<script setup>
import { defineProps, defineEmits, ref } from "vue";
import formatDate from "../functions/formatDate";

const props = defineProps({
  tableData: Array, // Define tableData as an array prop
});

const emit = defineEmits(["updateTableData"]); // Event to propagate the changes back to parent

// Store error messages and previous valid values
const errors = ref(Array(props.tableData.length).fill(""));
const previousValues = ref(
  props.tableData.map((item) => ({
    ENTSOE_DE_DAM_Price: item.ENTSOE_DE_DAM_Price,
    ENTSOE_GR_DAM_Price: item.ENTSOE_GR_DAM_Price,
    ENTSOE_FR_DAM_Price: item.ENTSOE_FR_DAM_Price,
  }))
);

// Validation function
const validateInput = (event, item, index, key) => {
  let value = event.target.value.trim();

  // Allow "-" when the user starts typing a negative number
  if (value === "-") {
    errors.value[index] = "Please enter a valid number.";
    event.target.value = previousValues.value[index][key]; // Restore previous valid value
    return;
  }

  let numericValue = Number(value);

  // Validation checks
  if (isNaN(numericValue) || numericValue < -2000 || numericValue > 2000) {
    errors.value[index] = "Value must be between -2000 and 2000.";
    event.target.value = previousValues.value[index][key]; // Restore previous valid value
    return;
  }

  // If valid, update the value and clear error message
  errors.value[index] = "";
  item[key] = numericValue;
  previousValues.value[index][key] = numericValue; // Store the last valid value

  emit("updateTableData", props.tableData);
};
</script>

<template>
  <div class="table-wrapper">
    <label for="table">Timeseries Table</label>
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th>Timestamp</th>
            <th>ENTSOE_DE_DAM_Price</th>
            <th>ENTSOE_GR_DAM_Price</th>
            <th>ENTSOE_FR_DAM_Price</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in props.tableData" :key="index">
            <td>
              <input type="text" :value="formatDate(item.DateTime)" readonly />
            </td>
            <td>
              <div class="input-container">
                <input
                  type="number"
                  :value="item.ENTSOE_DE_DAM_Price"
                  @input="validateInput($event, item, index, 'ENTSOE_DE_DAM_Price')"
                />
                <span v-if="errors[index]" class="error-message">{{
                  errors[index]
                }}</span>
              </div>
            </td>
            <td>
              <div class="input-container">
                <input
                  type="number"
                  :value="item.ENTSOE_GR_DAM_Price"
                  @input="validateInput($event, item, index, 'ENTSOE_GR_DAM_Price')"
                />
                <span v-if="errors[index]" class="error-message">{{
                  errors[index]
                }}</span>
              </div>
            </td>
            <td>
              <div class="input-container">
                <input
                  type="number"
                  :value="item.ENTSOE_FR_DAM_Price"
                  @input="validateInput($event, item, index, 'ENTSOE_FR_DAM_Price')"
                />
                <span v-if="errors[index]" class="error-message">{{
                  errors[index]
                }}</span>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
label {
  display: block;
  text-align: center;
  background: rgba(0, 162, 255, 0.2);
  color: #00a2ff;
  border-radius: 8px;
  border: 2px solid rgba(0, 162, 255, 0.5);
  box-shadow: 0 4px 10px rgba(0, 162, 255, 0.3);
  backdrop-filter: blur(10px);
  font-weight: bold;
}

.table-wrapper {
  width: 100%;
  max-width: 850px;
  overflow-x: auto;
  margin: 10px auto;
  border: 1px solid #acacacad;
  padding: 5px;
  border-radius: 8px;
  background: white;
}

.table-container {
  max-height: 70vh;
  overflow-y: auto;
  display: block;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  position: sticky;
  top: 0;
  background-color: #f1f1f1;
  z-index: 10;
}

th,
td {
  text-align: center;
  padding: 5px;
  font-size: 12px;
}

th {
  background-color: rgba(117, 211, 255, 0.3);
}

.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  text-align: center;
  width: 100%;
  padding: 3px;
  font-size: 12px;
}

input[type="number"] {
  text-align: right;
}

.error-message {
  color: red;
  font-size: 10px;
  margin-top: 2px;
}

tr:nth-child(even) {
  background-color: rgb(156, 206, 253);
}

tr:nth-child(odd) {
  background-color: rgb(184, 215, 255);
}

@media screen and (max-width: 1400px) {
  .table-wrapper {
    width: 95%;
  }
  table {
    font-size: 10px;
  }
  td,
  th {
    padding: 4px;
  }
}

@media screen and (max-width: 1200px) {
  .table-wrapper {
    overflow-x: auto;
    width: 100%;
  }
}

@media screen and (max-width: 768px) {
  table {
    font-size: 9px;
  }
  td,
  th {
    padding: 3px;
  }
  input {
    padding: 2px;
    font-size: 9px;
    width: fit-content;
  }
}

@media screen and (max-width: 500px) {
  .table-wrapper {
    padding: 3px;
  }
}
</style>
