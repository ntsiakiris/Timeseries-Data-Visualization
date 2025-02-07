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
                   @input="handleDateChange(index, $event.target.value)" />
          </td>
          <td>
            <input type="number" v-model="item.ENTSOE_DE_DAM_Price" />
          </td>
          <td>
            <input type="number" v-model="item.ENTSOE_GR_DAM_Price" />
          </td>
          <td>
            <input type="number" v-model="item.ENTSOE_FR_DAM_Price" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.table-container {
  margin: 20px;
  display: flex;
  justify-content: center;
  align-content: center;
}

table {
  border-collapse: collapse;
  width: 100%;
  max-width: 700px;
}

th, td {
  text-align: center;
  padding: 10px;
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
</style>
