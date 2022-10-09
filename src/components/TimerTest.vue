<template>
  <h2>
    Stopwatch
  </h2>

  <p>{{ time }}</p>

  <button @click="btnName === 'Start' ? start() : stop()">{{ btnName }}</button>
  <button @click="reset">Reset</button>
</template>

<script>
export default {
  name: 'timerTest',
  data() {
    return {
      btnName: 'Start',
      time: '',
      running: false,
      started: '',
      timeBegan: '',
      timeStopped: '',
      stoppedDuration: 0
    }
  },
  methods: {
    start() {
      if (this.running) return;
      if (this.timeBegan === null) {
        this.reset();
        this.timeBegan = new Date();
      }

      if (this.timeStopped !== null) {
        this.stoppedDuration += (new Date() - this.timeStopped);
      }

      this.started = setInterval(this.clockRunning, 10);
      this.running = true;
      this.btnName = 'Stop'
    },
    stop() {
      this.running = false;
      this.timeStopped = new Date();
      clearInterval(this.started);
      this.btnName = 'Start';
    },
    reset() {
      this.running = false;
      clearInterval(this.started);
      this.stoppedDuration = 0;
      this.timeBegan = null;
      this.timeStopped = null;
      this.time = "00:00:00.000";
      this.btnName = 'Start';
    },
    clockRunning() {
      const currentTime = new Date();
      const timeElapsed = new Date(currentTime - this.timeBegan - this.stoppedDuration);
      const hour = timeElapsed.getUTCHours(), min = timeElapsed.getUTCMinutes(), sec = timeElapsed.getUTCSeconds();
      const ms = timeElapsed.getUTCMilliseconds();

      this.time =
          this.zeroPrefix(hour, 2) + ":" +
          this.zeroPrefix(min, 2) + ":" +
          this.zeroPrefix(sec, 2) + "." +
          this.zeroPrefix(ms, 3);
    },
    zeroPrefix(num, digit) {
      let zero = '';
      for (let i = 0; i < digit; i++) {
        zero += '0';
      }
      return (zero + num).slice(-digit);
    }
  }
}

</script>