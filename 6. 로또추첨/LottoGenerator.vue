<template>
  <div>
    <div>당첨 숫자</div>
    <div id="resultScreen">
      <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
    </div>
    <div>보너스</div>
    <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
    <button v-if="redo" @click="onClickRedo">한 번 더!</button>
  </div>
</template>

<script>
  import LottoBall from "./LottoBall";

  const timeouts = [];
  function getWinNumbers() {
    const candidate = Array(45).fill().map((v, i) => i + 1);
    const shuffle = [];
    while (candidate.length > 0) {
      shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
    }
    const bonusNumber = shuffle[shuffle.length - 1];
    const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
    return [...winNumbers, bonusNumber];
  }

  export default {
    components: {
      'lotto-ball' : LottoBall
    },
    data() {
      return {
        winNumbers: getWinNumbers(),
        winBalls: [],
        bonus: null,
        redo: false,
      }
    },
    methods: {
      showBalls() {
        for (let i = 0; i < this.winNumbers.length - 1 ; i++) {
          timeouts[i] = setTimeout(() => {
            this.winBalls.push(this.winNumbers[i]);
          }, (i + 1) * 1000);
        }
        timeouts[6] = setTimeout(() => {
          this.bonus = this.winNumbers[6];
          this.redo = true;
        }, 7000);
      },
      onClickRedo() {
        this.winBalls = [];
        this.bonus = null;
        this.redo = false;
        this.showBalls();
      }
    },
    mounted() {
      this.showBalls();
    },
    beforeDestroy() {
      timeouts.forEach((t) => {
        clearTimeout(t);
      });
    }
  };
</script>

<style scoped>

</style>