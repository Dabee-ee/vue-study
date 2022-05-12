<template>
  <td @click="onClickTd">{{cellData}}</td>
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
    methods: {
      onClickTd() {
        if (this.cellData) return;

        const rootData = this.$root.$data;
        console.log(rootData);
        this.$set(rootData.tableData[this.rowIndex], this.cellIndex, rootData.turn); // 클릭한 셀

        // 승부 판단하기
        let win = false;
        // 가로줄 검사
        if (rootData.tableData[this.rowIndex][0] === rootData.turn && rootData.tableData[this.rowIndex][1] === rootData.turn && rootData.tableData[this.rowIndex][2] === rootData.turn) {
          win = true;
        }
        // 세로줄 검사
        if (rootData.tableData[0][this.cellIndex] === rootData.turn && rootData.tableData[1][this.cellIndex] === rootData.turn && rootData.tableData[2][this.cellIndex] === rootData.turn) {
          win = true;
        }
        // 대각선 검사
        if (rootData.tableData[0][0] === rootData.turn && rootData.tableData[1][1] === rootData.turn && rootData.tableData[2][2] === rootData.turn) {
          win = true;
        }
        if (rootData.tableData[0][2] === rootData.turn && rootData.tableData[1][1] === rootData.turn && rootData.tableData[2][0] === rootData.turn) {
          win = true;
        }


        if (win) { // 이긴 경우: 3줄 달성 (승자가 있는 경우)
          rootData.winner = rootData.turn;
          rootData.turn = 'O';
          rootData.tableData = [['', '', ''], ['', '', ''], ['', '', '']];
        } else { // 무승부 (승자가 없는 경우)
          let all = true; // all이 true면 무승부라는 뜻

            rootData.tableData.forEach((row) => { // 무승부 검사 ( 반복문으로 칸을 돌면서 칸이 찼는지 확인 --> 칸이 다 차있으면 all이 true )
              row.forEach((cell) => {
                if (!cell) {
                  all = false;
                }
              });
            });

            if (all) { // 무승부
              rootData.winner = '';
              rootData.turn = 'O';
              rootData.tableData = [['', '', ''], ['', '', ''], ['', '', '']];
            } else {
              rootData.turn = rootData.turn === 'O' ? 'X' : 'O';
            }
        }

      }
    }
  };
</script>

<style scoped>

</style>