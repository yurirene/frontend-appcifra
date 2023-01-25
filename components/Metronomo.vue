<template>
  <div>
    <em style="font-size:20px;" class="me-2 fas fa-clock"></em>
    <button class="start-stop" style="font-size: 20px;" @click="playBpm">
      <em :class="`fas ${isRunning ? 'fa-stop text-danger' : 'fa-play text-success'}`"></em>
    </button>
  </div>
</template>
<script>
export default {
  props: {
    batimento: {
      type: Number,
      default: 140
    }
  },
  data() {
    return {
      metronome: null,
      beatsPerMeasure: 4,
      count: 0,
      isRunning: false,
      bpm: this.batimento
    }
  },
  mounted() {
    this.metronome = new this.Timer(this.playClick, 60000 / this.bpm, { immediate: true });
  },
  watch: {
    batimento: function(value) {
      this.bpm = value;
      this.metronome = new this.Timer(this.playClick, 60000 / this.bpm, { immediate: true });
    }
  },
  methods: {
    playBpm: function() {
      this.count = 0;
      if (!this.isRunning) {
          this.metronome.start();
          this.isRunning = true;
      } else {
          this.metronome.stop();
          this.isRunning = false;
      }
    },
    playClick: function () {
        const click = new Audio('/assets/click/click2.mp3');

        if (this.count === this.beatsPerMeasure) {
            this.count = 0;
        }
        click.play();
        click.currentTime = 0;
        this.count++;
    },

  }

}
</script>
