<template>
  <tr>
    <td-component v-for="(cellData, index) in rowData" :cell-data="cellData" :cell-idnex="index" :row-index="rowIndex"></td-component>
  </tr>
</template>

<script>
  import TdComponent from "./TdComponent";

  export default {
    props: {
      rowData: Array,
      rowIndex: Number
    },
    components: {
      TdComponent
    },
    data() {
      return {

      }
    },
    computed: {},
    methods: {}
  };
</script>

<style scoped>

</style>