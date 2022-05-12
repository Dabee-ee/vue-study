<template>
  <td @click="onClickTd">{{cellData}}</td>
</template>

<script>
import EventBus from './EventBus';

export default {
  props: {
    cellData: String,
    rowIndex: Number,
    cellIndex: Number,
  },
  methods: {
    onClickTd() {
      if (this.cellData) return;

      // emit은 on과 대응된다. 연결된 핸들러가 실행된다.
      // 이벤트 버스를 사용하면, 루트 컴포넌트에서 모든 데이터 관련 작업을 처리해줄 수 있는 것이 장점이다.
      EventBus.$emit('clickTd', this.rowIndex, this.cellIndex);
    }
  }
};
</script>