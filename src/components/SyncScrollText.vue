<template>
  <div class="">
    <slot></slot>
  </div>
</template>

<script>


export default {
  name: 'SyncScrollText',
  props: {
  },
  data() {
    return {
    };
  },
  mounted() {
    var minBreakTime = 2000;
    var pixelPerSecond = 200;

    var children = this.$children.filter((child) => { return child.$options.name === 'ScrollText'});
    var highestDuration = 0;
    children.forEach((c) => {
      var duration = c.$data.textWidth / pixelPerSecond;
      if (duration > highestDuration) {
        highestDuration = duration;
      }
    });

    // set pause time of each
    setTimeout(() => {
      children.forEach((c) => {
        var duration = c.$data.textWidth / pixelPerSecond;
        var diff = highestDuration - duration;
        var breakTime = minBreakTime + (diff * 1000);
        c.startScrolling(breakTime, pixelPerSecond);
      });
    }, 2000)


  },
  methods: {

  }
}


</script>

<style scoped>

</style>
