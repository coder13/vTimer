<template>
  <div class="v-timer">
    <span id="time" :class="{preStart: (started === null && isDown), 'postStart': (started !== null && isDown)}">{{time | pretty}}</span>
  </div>
</template>

<script>
const pad = n => (n < 10 ? '0' : '') + n;
const fixed = (n, d) => Number(n).toFixed(d === undefined ? 2 : d);

function now() {
  return (window.performance && window.performance.now
    ? window.performance.now.bind(window.performance)
    : Date.now)().toFixed();
}

const requestAnimationFrame =
    window.requestAnimationFrame || window.webkitRequestAnimationFrame ||
    window.mozRequestAnimationFrame || window.oRequestAnimationFrame ||
    window.msRequestAnimationFrame ||
    function request(fn) { return window.setTimeout(fn, 1000 / 60); };

const cancelAnimationFrame =
    window.cancelAnimationFrame || window.webkitCancelAnimationFrame ||
    window.mozCancelRequestAnimationFrame ||
    window.oCancelRequestAnimationFrame ||
    window.msCancelRequestAnimationFrame || window.clearTimeout;

function setInterval(fn) {
  // Have to use an object here to store a reference
  // to the requestAnimationFrame ID.
  const handle = {};

  function interval() {
    fn.call();
    handle.value = requestAnimationFrame(interval);
  }

  handle.value = requestAnimationFrame(interval);
  return handle;
}

function clearInterval(interval) {
  cancelAnimationFrame(interval.value);
}

export default {
  name: 'Timer',
  template: '<div @click.space="start">{{time | pretty}}</div>',
  created() {
    window.addEventListener('keydown', (event) => {
      if (this.focused && event.keyCode === 32) {
        this.down();
      }
    });

    window.addEventListener('keyup', (event) => {
      if (this.focused && event.keyCode === 32) {
        this.up();
      }
    });
  },
  methods: {
    down() {
      this.isDown = true;

      if (this.started) {
        this.stop();
      }
    },
    up() {
      this.isDown = false;

      if (!this.started) {
        this.start();
      } else {
        this.started = null;
      }
    },
    start() {
      this.time = 0;
      this.started = now();
      this.timerObj = setInterval(this.tick, 10);
    },
    stop() {
      if (this.timerObj) {
        clearInterval(this.timerObj);
      }
    },
    tick() {
      this.time = (now() - this.started);
    },
  },
  data() {
    return {
      focused: true,
      started: null,
      isDown: false,
      timerObj: null,
      time: 0,
    };
  },
  filters: {
    pretty(time) {
      let s = time / 1000;
      const hours = Math.floor(s / 3600);
      s %= 3600;
      const minutes = Math.floor(s / 60);
      const seconds = fixed(s % 60, (minutes + (hours * 60)) > 10 ? 0 : 2);

      return `${hours ? `${hours}:` : ''}${minutes ? `${hours ? pad(minutes, '0') : minutes}:` : ''}${minutes ? pad(seconds, '0') : seconds}`;
    },
  },
};
</script>

<style scoped>
#time {
  font-size: 15em;
}

.preStart {
  color: green;
}

.postStart {
  color: red;
}
</style>