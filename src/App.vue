<template>
  <v-app>
    <v-main class="pt-4">
      <break-reminder v-if="remind" :message="message" :timeout="timeout - elapsed" :paused="paused" @onpause="handlePause" @oncontinue="handleContinue" @onskip="handleSkip" />
    </v-main>
  </v-app>
</template>

<script>
import BreakReminder from './components/BreakReminder';
const { ipcRenderer } = require('electron');

export default {
  name: 'App',
  components: {
    BreakReminder
  },
  data() {
    return {
      ticker: null,
      elapsed: 0,
      timeout: 20,
      activityLength: 600,
      remind: false,
      paused: false,
      message: 'Time to take a break'
    }
  },
  mounted() {
    this.ticker = this.startTimer();
  },
  methods: {
    startTimer() {
      this.paused = false;
      return setInterval(() => {
        this.elapsed++;

        if(!this.remind && this.elapsed === this.activityLength)
          this.show();

        if(this.remind && this.elapsed === this.timeout)
          this.hide()

      }, 1000);
    },
    handleSkip() {
      this.hide();
    },
    handlePause() {
      clearInterval(this.ticker);
      this.paused = true;
    },
    handleContinue() {
      this.ticker = this.startTimer();
    },
    show() {
      this.elapsed = 0;
      this.remind = true;
      ipcRenderer.send('onshow');
    },
    hide() {
      this.elapsed = 0;
      this.remind = false;
      ipcRenderer.send('onhide');
    }
  }
}
</script>