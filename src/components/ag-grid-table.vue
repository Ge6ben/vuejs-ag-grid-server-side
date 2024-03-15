<template>
     <!-- The AG Grid component -->
    <ag-grid-vue
      :rowData="props.rowData"
      :columnDefs="props.colDefs"
      style="  height: calc(100vh - 290px)"
      class="ag-theme-quartz"
    >
    </ag-grid-vue>

 </template>
<script setup>
import "ag-grid-community/styles/ag-grid.css"; // Mandatory CSS required by the grid
import "ag-grid-community/styles/ag-theme-quartz.css"; // Optional Theme applied to the grid
import { AgGridVue } from "ag-grid-vue3"; // AG Grid Component
const props = defineProps(['rowData' ,'colDefs'])


</script>

