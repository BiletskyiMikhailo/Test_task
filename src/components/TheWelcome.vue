
<template>
  <div>
    <ag-grid-vue
    style="width: 1600px; height: 1024px"
    class="ag-theme-alpine"
     :columnDefs="columnDefs"
     :rowData="generatedObjects" 
     :defaultColDef="defaultColDef"
      :gridOptions="gridOptions" 
      @gridReady="onGridReady">
    </ag-grid-vue>
  </div>
</template>
<script>
import "ag-grid-community/styles//ag-grid.css";
import "ag-grid-community/styles//ag-theme-alpine.css";
import { AgGridVue } from "ag-grid-vue3";

export default {
  components: {
    AgGridVue
  },
  data() {
    return {
      generatedObjects: [],
      columnDefs: [
        {
          headerName: "GROUP1", children: [
            { headerName: "Col1", field: "val6", cellRenderer: this.renderImg },
            { headerName: "Col2", field: "val1" },
            { headerName: "Col3", field: "val2", valueFormatter: this.addKg },
          ]
        },
        {
          headerName: "GROUP2", children: [
            { headerName: "Col4", field: "val4" },
            { headerName: "Col5", field: "val7", cellRenderer: this.renderBoldUnderline },
            { headerName: "Col6", field: "val8", filter: 'text', filterParams: { newRowsAction: 'keep' } },
          ]
        },
        {
          headerName: "Summary", children: [
            { headerName: "Sum of Col3", field: "val2", valueFormatter: this.addKg, cellRenderer: "agAnimateShowChangeCellRenderer", cellRendererParams: { innerRenderer: 'agGroupCellRenderer' }, aggFunc: "sum" },
            { headerName: "Avg of Col4", field: "val4", cellRenderer: "agAnimateShowChangeCellRenderer", cellRendererParams: { innerRenderer: 'agGroupCellRenderer' }, aggFunc: "avg" },
          ]
        },
      ],
      defaultColDef: {
        sortable: true,
        filter: true
      },
      gridOptions: {
        onRowClicked: this.onRowClicked,
        rowGroupPanelShow: 'always',
        pivotPanelShow: 'never',
        suppressAggFuncInHeader: true,
        suppressRowClickSelection: true,
        suppressCellSelection: true,
      }
    }
  },
  created() {
    this.generatedObjects = this.generateObjects();
  },
  methods: {
    generateObjects() {
      let objects = [];
      let strings = ["str1", "str2", "str3", "str4", "str5"];
      for (let i = 0; i < 100; i++) {
        let obj = {
          val1: Math.random().toString(36).substring(2, 12),
          val2: (Math.random() * 100).toFixed(2),
          val3: (Math.random() * 10000).toFixed(4),
          val4: Math.floor(Math.random() * 100),
          val5:  Math.floor(Math.random() * 100),
          val6: "https://via.placeholder.com/150",
          val7: Math.random().toString(36).substring(2, 12),
          val8: strings[Math.floor(Math.random() * 5)]
        };
        objects.push(obj);
      }
      return objects;
    },
    renderImg(params) {
      return `<img src="${params.value}" width="50" height="50" />`;
    },
    addKg(params) {
      return `${ params.value } кг`;
    },
    renderBoldUnderline(params) {
      return `<b><u>${params.value}</u></b>`;
    },
    onGridReady(params) {
      params.api.sizeColumnsToFit();
    },
    onRowClicked(params) {
      if (params.node.group) {
        return;
      }
      alert(JSON.stringify(params.data));
    }
  }
}
</script>
