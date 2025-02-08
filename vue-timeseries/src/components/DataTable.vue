<script setup>
import { defineProps, ref } from 'vue';

const props = defineProps(['tableData']);

const tableData = ref(props.tableData);

const formatDate = (dateString) => {
  return new Date(dateString).toLocaleString('en-GB', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  });
};

const handleDateChange = (index, newDate) => {
  tableData.value[index].DateTime = new Date(newDate).toISOString();  // edit date in the correct format
};

//  input number validation
const validateInput = (event, item) => {
  let value = event.target.value;

  // Allow "-" when user starts typing a negative number
  if (value === "-") {
    item.ENTSOE_DE_DAM_Price = value;
    return;
  }

  // Convert to number
  let numericValue = Number(value);

  // If valid number, clamp within range
  if (!isNaN(numericValue)) {
    item.ENTSOE_DE_DAM_Price = Math.min(2000, Math.max(-2000, numericValue));
  } else {
    // Reset to min or max if out of range
    item.ENTSOE_DE_DAM_Price = "";
  }
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
        <tr v-for="(item, index) in tableData" :key="index">
          <td>
            <!-- Display formatted date -->
            <input type="text" 
            :value="formatDate(item.DateTime)" 
            readonly />
          </td>
          <td>
            <input type="number" 
            v-model="item.ENTSOE_DE_DAM_Price"
            @input="validateInput($event, item)" />
          </td>
          <td>
            <input type="number" 
            v-model="item.ENTSOE_GR_DAM_Price"
            @input="validateInput($event, item)" />
          </td>
          <td>
            <input type="number" 
            v-model="item.ENTSOE_FR_DAM_Price" 
            @input="validateInput($event, item)"/>
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

th, td {
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

input {
  text-align: center;
  width: 100%;
  padding: 5px;
  border-radius: 4px;
}

input[type="number"] {
  text-align: right;
}


tr:nth-child(even) {
  background-color: rgb(187, 187, 187);
}

tr:nth-child(odd) {
  background-color: rgb(142, 191, 255);
}
</style>
