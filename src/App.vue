<template>
  <div>
    <AGGridTable :colDefs="colDefs"  :rowData="rowData"></AGGridTable>
  </div>
</template>
<script setup>
import 'ag-grid-enterprise';
import { LicenseManager } from 'ag-grid-enterprise';
 import AGGridTable from './components/ag-grid-table.vue'
import { onBeforeMount, ref } from 'vue'
onBeforeMount(()=>{
  LicenseManager.setLicenseKey(import.meta.env.AG_GRID_LICENSE_KEY  );
})


const rowData = ref([
  { make: "Tesla", model: "Model Y", price: 64950, electric: true },
  { make: "Ford", model: "F-Series", price: 33850, electric: false },
  { make: "Toyota", model: "Corolla", price: 29600, electric: false },
]);

const colDefs = ref([
  { field: "make" },
  { field: "model" },
  { field: "price" },
  { field: "electric" }
]);

 </script>

