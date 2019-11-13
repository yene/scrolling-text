<template>
  <div id="app">
    <SyncScrollText :speed="150" class="scroll-text-container">
      <ScrollText text="Soundtrack with a long name, feat. Leila Adu" class="song-name"/>
      <ScrollText text="Artist with a very long, looooong name, Alan Walker, Gen Hoshino" class="artist-name"/>
    </SyncScrollText>
  </div>
</template>

<script>
import ScrollText from './components/ScrollText.vue'
import SyncScrollText from './components/SyncScrollText.vue'

export default {
  name: 'app',
  components: {
    ScrollText,
    SyncScrollText,
  }
}
</script>

<style>
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
html, body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-size: 28px;
  line-height: 1em;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  margin-left: 10px;
  width: 590px;
  height: 455px;
  background-image: url('/background.png');
}
.scroll-text-container {
  position: relative;
  margin-left: 40px;
  margin-right: 140px;
  padding-top: 56px;
}

.song-name {
  font-weight: bold;
}
.artist-name {
  color: #D90053;
}
</style>
