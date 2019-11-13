<template>
  <div class="">
    <slot></slot>
  </div>
</template>

<script>


export default {
  name: 'SyncScrollText',
  props: {
    speed: {
      type: Number,
      default: 200,
    },
  },
  mounted() {
    var minBreakTime = 2000;
    var children = this.$children.filter((child) => { return child.$options.name === 'ScrollText'});
    var highestDuration = 0;
    children.forEach((c) => {
      var duration = c.$data.textWidth / this.speed;
      if (duration > highestDuration) {
        highestDuration = duration;
      }
    });

    // set pause time of each
    setTimeout(() => {
      children.forEach((c) => {
        var duration = c.$data.textWidth / this.speed;
        var diff = highestDuration - duration;
        var breakTime = minBreakTime + (diff * 1000);
        c.startScrolling(breakTime, this.speed);
      });
    }, minBreakTime);
  }
}

</script>
