<template>
  <div ref="scrollText" class="scroll-text">
    <div ref="container" class="container stopped">
      <span ref="first">{{ text }}</span><span ref="second" class="hidden follower">{{ text }}</span>
    </div>
  </div>
</template>

<script>

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
    var containerWidth = this.$refs.scrollText.clientWidth;
    var textWidth = this.$refs.container.clientWidth - 20; // we have to subtract the spacing between

    if (textWidth < containerWidth) {
      return;
    }
    this.textWidth = textWidth;

    // Restarting of a CSS animation is done by overwriting the animatino property and then freeing it again for the inherited to trigger
    this.$refs.container.addEventListener('animationend', () => {
      this.$refs.container.classList.add('stopped');
      setTimeout(this.startScrolling, this.breakTime);
    });
  },
  methods: {
    // The parent (SyncScrollText) calulcates the wait time for the children, and calls this method directly.
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
      var follower = this.$refs.second;
      follower.classList.remove('hidden');
      this.$refs.container.style['animation-duration'] = this.duration + 's';
      this.$refs.container.classList.remove('stopped');
    },
  }
}

function browserCanUseCssVariables() {
  return window.CSS && CSS.supports('color', 'var(--fake-var)');
}

</script>

<style scoped>
.stopped {
  animation: none !important;
}
.hidden {
  display: none !important;
}
.scroll-text {
  margin: 0 auto;
  white-space: nowrap;
  overflow: hidden;
  box-sizing: border-box;
  padding: 0;
  display: block;
}
.container {
  display: inline-block;
  text-indent: 0;
  overflow: hidden;
  animation: marquee 5s linear infinite;
  animation-iteration-count: 1;
}
.container span {
  padding-left: 40px;
  padding-right: 10px; /* space between spans */
}

.scroll-text::before {
  z-index: 1;
  background-image: linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(0,0,0,0) 7%, rgba(0,0,0,0) 93%, rgba(255,255,255,1) 100%);
  content: "\00a0";
  position: absolute;
  width: 100%;
  left: 0px;
}

@keyframes marquee {
  0% { transform: translate(0, 0);}
  100% { transform: translate(-50%, 0);}
}

</style>
