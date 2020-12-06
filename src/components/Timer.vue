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
      }
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
          clearInterval(this.timerId);
          break;
        case 'reset':
          clearInterval(this.timerId);
          this.timerValue = '00:00:00';
          this.startTimer();
          break;
      }
    }

    startTimer() {
      const startTimestamp = moment().startOf("day");
      this.timerId = setInterval(() => {
        startTimestamp.add(1, 'second');
        this.timerValue = startTimestamp.format('HH:mm:ss');
      }, 1000);
    }
  }
</script>

<style scoped>

</style>
