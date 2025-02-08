<script setup>
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps({
  tableData: Array, // Define tableData as an array prop
});

const emit = defineEmits(["updateTableData"]); // Event to propagate the changes back to parent

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleString("en-GB", {
    day: "2-digit",
    month: "2-digit",
    year: "numeric",
    hour: "2-digit",
    minute: "2-digit",
  });
};

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

  // Emit updated data
  emit("updateTableData", props.tableData);
};
</script>

<template>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Timestamp</th>
          <th>DE Price (€)</th>
          <th>GR Price (€)</th>
          <th>FR Price (€)</th>
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
              <span v-if="errors[index]" class="error-message">{{ errors[index] }}</span>
            </div>
          </td>
          <td>
            <div class="input-container">
              <input
                type="number"
                :value="item.ENTSOE_GR_DAM_Price"
                @input="validateInput($event, item, index, 'ENTSOE_GR_DAM_Price')"
              />
              <span v-if="errors[index]" class="error-message">{{ errors[index] }}</span>
            </div>
          </td>
          <td>
            <div class="input-container">
              <input
                type="number"
                :value="item.ENTSOE_FR_DAM_Price"
                @input="validateInput($event, item, index, 'ENTSOE_FR_DAM_Price')"
              />
              <span v-if="errors[index]" class="error-message">{{ errors[index] }}</span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.table-container {
  margin: 20px;
}

table {
  border-collapse: collapse;
  width: 100%;
  max-width: 600px;
  border-collapse: collapse;
  border: 1px solid #3d3d3d;
}

th,
td {
  text-align: center;
  padding: 3px;
  border: 1px solid #3d3d3d;
}

th {
  background-color: #f1f1f1;
}

tr:hover {
  background-color: #f9f9f9;
}

.input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  text-align: center;
  width: 100%;
  padding: 5px;
  border-radius: 4px;
}

input[type="number"] {
  text-align: right;
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 2px;
}

tr:nth-child(even) {
  background-color: rgb(187, 187, 187);
}

tr:nth-child(odd) {
  background-color: rgb(142, 191, 255);
}
</style>
