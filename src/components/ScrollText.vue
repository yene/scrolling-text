<template>
  <div class="scroll-text">
    <div ref="scrollBox" class="container margin-transition stopped"><span ref="scrollTextBase" class="base">{{ text }}</span><span ref="scrollTextFollower" class="hidden follower">{{ text }}</span></div>
    <button v-on:click="startScrolling">test</button>
  </div>
</template>

<script>
/* Notes
* CSS animations does not allow for breaks between iterations, you could implement it by adding keyframes with no changes.
* To restart animation remove/add a pause class or remove/add a class which sets animation: none!important. Going with animationend over animationiterate is more precise.

*/


let breakTime = 2; // in s
// let speed = 5; // characters per s

export default {
  name: 'ScrollText',
  props: {
    text: String,
  },
  data() {
    return {

    };
  },
  mounted() {
    if (!browserCanUseCssVariables()) {
      alert('Your browser does not support CSS Variables and/or CSS.supports. :-(');
      return;
    }
    var containerWidth = this.$refs.scrollBox.clientWidth;
    var textWidth = this.$refs.scrollTextBase.clientWidth;
    if (textWidth < containerWidth) {
      return;
    }
    var follower = this.$refs.scrollTextFollower;
    follower.classList.remove('hidden');

    // var animationmargin = getComputedStyle(this.$refs.scrollBox).getPropertyValue('--animation-margin'); if this fails we have no var support
    this.$refs.scrollBox.style.setProperty('--animation-margin', '-' + (textWidth + 20) + 'px');

  },
  methods: {
    startScrolling() {
      this.$refs.scrollBox.classList.remove('stopped');
      this.$refs.scrollBox.addEventListener('animationend', () => {
        this.$refs.scrollBox.classList.add('stopped');
        setTimeout(this.startScrolling, breakTime * 1000);
      });
    },
    pauseScrolling() {

    },
    stopScrolling() {

    },
  }
}

function browserCanUseCssVariables() {
  return window.CSS && CSS.supports('color', 'var(--fake-var)');
}

</script>

<style scoped>
.stopped {
  /* animation-play-state: paused !important; */
  animation: none !important;
}
.hidden {
  display: none;
}
.follower {
  margin-left: 20px;
}
.scroll-text {
  --animation-margin: -20px;

  white-space: nowrap;
  overflow: hidden;
}
.scroll-text span {
  display: inline-block;
}
.margin-transition {
  animation-name: animate-margin;
  animation-duration: 5s;
  animation-timing-function: linear;
  animation-delay: 0s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-fill-mode: none;
  animation-play-state: running;
}

@keyframes animate-margin {
  0% { margin-left: 0; }
  100% { margin-left: var(--animation-margin); }
}
</style>
