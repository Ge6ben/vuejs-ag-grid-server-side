<template>
  <div>
    <AGGridTable :colDefs="colDefs" @on-grid-ready="onGridReady" />
  </div>
</template>
<script setup>
import 'ag-grid-enterprise'
import { LicenseManager } from 'ag-grid-enterprise'
import AGGridTable from './components/ag-grid-table.vue'
import { onBeforeMount, ref } from 'vue'

onBeforeMount(() => {
  LicenseManager.setLicenseKey(import.meta.env.AG_GRID_LICENSE_KEY)
})


let total = ref(0)
const gridApi = ref()

const onGridReady = async (params) => {
  gridApi.value = params.api
  const datasource = createServerSideDatasource()
  await gridApi.value.setGridOption('serverSideDatasource', datasource)
}

function createServerSideDatasource() {
  return {
    getRows: async (params) => {
      gridApi.value.hideOverlay()

      let startRow = params.request.startRow
      let endRow = params.request.endRow

      fetchData(startRow, endRow)
        .then((data) => {
           params.success({ rowData: data, rowCount: total })
        })
        .catch(() => {
          params.fail()
        })
    }
  }
}

function fetchData(startRow, endRow) {
  return new Promise((resolve, reject) => {
    fetch('https://www.ag-grid.com/example-assets/olympic-winners.json')
      .then((resp) => resp.json())
      .then((data) => {
        data = data.map((i, index) => {
          return { ...i, id: index }
        })
        total = data.length
        const dataWithApplyPagination = data.slice(startRow, endRow)
        resolve(dataWithApplyPagination)
      })
      .catch((error) => {
        reject(error)
      })
  })
}

const colDefs = ref([
  { field: 'id', headerName: 'Number' },
  { field: 'athlete' },
  { field: 'country' },
  { field: 'year' },
  { field: 'sport' },
  { field: 'gold' },
  { field: 'silver' },
  { field: 'bronze' }
])
</script>
