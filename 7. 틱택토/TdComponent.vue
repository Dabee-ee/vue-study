<template>
  <td>{{cellData}}</td>
</template>

<script>
  export default {
    props: {
      cellData: String,
      rowIndex: Number,
      cellIndex: Number,
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