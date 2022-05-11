<template>
  <div>
    <h1>{{result}}</h1>
    <form @submit.prevent="onSubmitForm">
      <input ref="answer" minlength="4" maxlength="4" v-model="value" />
      <button type="submit">입력</button>
    </form>
    <div>시도: {{tries.length}}</div>
    <ul>
      <!-- TODO : 사용자가 중복 값을 입력했을 경우 key duplicate error. 이를 막아주는 처리 필요. -->
      <li v-for="t in tries" :key="t.try">
        <div>{{t.try}}</div>
        <div>{{t.result}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
  const getNumbers = () => {
    const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const array = [];
    for (let i = 0; i < 4; i++) {
      const chosen = candidates.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
      array.push(chosen);
    }
    return array;
  };

  export  default {
    data() {
      return  {
        answer: getNumbers(),
        tries : [],
        value : '',
        result : '',
      }
    },
    methods : {
      onSubmitForm(e) {
        if (this.value === this.answer.join('')) {
          // 정답을 맞췄을 경우
          this.tries.push({
            try : this.value,
            result : '홈런'
          });
          this.result = '홈런';
          alert('게임을 다시 실행합니다.');
          this.value = '';
          this.tries = [];
          this.answer = getNumbers();
          this.$refs.answer.focus();
        } else {
          // 오답일 경우

          // 10번째 시도
          if ( this.tries.length >= 9 ) {
            this.result = `시도 횟수 초과로 실패하였습니다. 답은 ${this.answer.join(',')}였습니다!`;
            alert('게임을 다시 시작합니다.');
            this.value = '';
            this.answer = getNumbers();
            this.tries = [];
            this.$refs.answer.focus();
          }
          let strike = 0;
          let ball = 0;
          const answerArray = this.value.split('').map(v => parseInt(v));

          for (let i = 0; i < 4; i++) {
            if (answerArray[i] === this.answer[i]) {
              // 숫자와 자릴수 모두 정답인 경우 (홈런)
              strike++;
            } else if (this.answer.includes(answerArray[i])) {
              // 숫자만 정답인 경우 (볼)
              ball++;
            }
          }
          this.tries.push({
            try : this.value,
            result : `${strike} 스트라이크, ${ball} 볼입니다.`,
          });
          this.value = '';
          this.$refs.answer.focus();
        }
      }
    }
  }

</script>

<style>

</style>