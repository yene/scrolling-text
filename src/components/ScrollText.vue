<template>
  <div class="scroll-text">
    <div ref="scrollBox" class="container margin-transition stopped"><span ref="scrollTextBase" class="base">{{ text }}</span><span ref="scrollTextFollower" class="hidden follower">{{ text }}</span></div>
  </div>
</template>

<script>
/* Notes
* Animating cut off text does not work with transform, transform works on the view layer.
* CSS animations does not allow for breaks between iterations, you could implement it by adding keyframes with no changes.
* To restart animation remove/add a pause class or remove/add a class which sets animation: none!important. Going with animationend over animationiterate is more precise.

*/

export default {
  name: 'ScrollText',
  props: {
    text: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      textWidth: 0,
      duration: 5,
      breakTime: 2000, // in ms
      pixelPerSecond: 50,
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
    this.textWidth = textWidth;

  },
  methods: {
    // Passing the parameters to children over a method.
    startScrolling(breakTime, pixelPerSecond) {
      if (this.textWidth === 0) {
        return;
      }
      if (breakTime !== undefined) {
        this.breakTime = breakTime;
      }
      if (pixelPerSecond !== undefined) {
        this.pixelPerSecond = pixelPerSecond;
      }

      this.duration = this.textWidth / this.pixelPerSecond;

      var follower = this.$refs.scrollTextFollower;
      follower.classList.remove('hidden');

      this.$refs.scrollBox.style['animation-duration'] = this.duration + 's';

      // var animationmargin = getComputedStyle(this.$refs.scrollBox).getPropertyValue('--animation-margin'); if this fails we have no var support
      this.$refs.scrollBox.style.setProperty('--animation-margin', '-' + (this.textWidth + 20) + 'px');

      this.$refs.scrollBox.classList.remove('stopped');
      this.$refs.scrollBox.addEventListener('animationend', () => {
        this.$refs.scrollBox.classList.add('stopped');
        setTimeout(this.startScrolling, this.breakTime);
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
  display: none !important;
}
.follower {
  margin-left: 20px;
}
.scroll-text {
  padding-left: 20px;
  --animation-margin: -20px;

  white-space: nowrap;
  overflow: hidden;
}
.scroll-text::before {
  background-image: linear-gradient( right,
          rgba( 255, 255, 255, 0 ) 0%,
          rgba( 255, 255, 255, 1 ) 100% );
  content: "\00a0";
  position: absolute;
  width: 20px;
  left: 0px;
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
