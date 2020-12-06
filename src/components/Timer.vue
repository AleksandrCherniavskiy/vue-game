<template>
  <div>{{timerValue}}</div>
</template>

<script lang="ts">
  import {Vue, Options} from 'vue-class-component';
  import moment from 'moment';

  @Options({
    props: ['timerEvent'],
    watch: {
      timerEvent: {
        handler: 'onTimerEventChanged',
      },
    }
  })
  export default class Timer extends Vue {

    private timerValue = '00:00:00';
    private timerId: number | undefined;


    onTimerEventChanged(val: string) {
      switch (val) {
        case 'start':
          this.startTimer();
          break;
        case 'stop':
          this.stopTimer();
          break;
      }
    }

    startTimer() {
      this.timerValue = '00:00:00';
      const startTimestamp = moment().startOf("day");
      this.timerId = setInterval(() => {
        startTimestamp.add(1, 'second');
        this.timerValue = startTimestamp.format('HH:mm:ss');
      }, 1000);
    }

    stopTimer() {
      clearInterval(this.timerId);
    }
  }
</script>

<style scoped>

</style>
