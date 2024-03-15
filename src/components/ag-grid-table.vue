<template>
  <!-- The AG Grid component -->
  <ag-grid-vue
    :blockLoadDebounceMillis="1000"
    :cacheBlockSize="cacheBlockSize"
    :columnDefs="props.colDefs"
    :gridOptions="gridOptions"
    :maxBlocksInCache="maxBlocksInCache"
    :maxConcurrentDatasourceRequests="1"
    :pagination="true"
    :paginationPageSize="paginationPageSize"
    :paginationPageSizeSelector="[1000, 2000, 3000, 4000]"
    :popupParent="popupParent"
    :rowModelType="rowModelType"
    :sideBar="sideBar"
    class="ag-theme-quartz"
    style="height: 800px"
    @grid-ready="onGridReady($event)"
  />
</template>
<script setup>
import 'ag-grid-community/styles/ag-grid.css' // Mandatory CSS required by the grid
import 'ag-grid-community/styles/ag-theme-quartz.css' // Optional Theme applied to the grid
import { AgGridVue } from 'ag-grid-vue3'
import { onBeforeMount, ref } from 'vue' // AG Grid Component
const props = defineProps(['colDefs'])
const emits = defineEmits(['on-grid-ready', 'on-update-filter'])
defineOptions({
  inheritAttrs: false
})

const paginationPageSize = ref()
const cacheBlockSize = ref()
const rowModelType = ref()
const maxBlocksInCache = ref()
const gridOptions = ref()
const popupParent = ref()
const sideBar = ref()

onBeforeMount(() => {
  rowModelType.value = 'serverSide'
  popupParent.value = document?.body
  paginationPageSize.value = 1000
  cacheBlockSize.value = 1000
  maxBlocksInCache.value = 1000

  sideBar.value = {
    toolPanels: [
      {
        id: 'columns',
        labelDefault: 'Columns',
        labelKey: 'columns',
        toolPanel: 'agColumnsToolPanel',
        iconKey: 'columns'
      }
    ],
    defaultToolPanel: []
  }
})

function onGridReady(params) {
  emits('on-grid-ready', params)
}
</script>
