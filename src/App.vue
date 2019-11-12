<template>
  <div id="app">
    <SyncScrollText :speed="100" class="scroll-text-container">
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
  font-size: 24px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  margin-left: 10px;
  width: 554px;
  height: 401px;
  background-image: url('/background.png');
}
.scroll-text-container {
  position: relative;
  margin-left: 35px;
  margin-right: 120px;
  padding-top: 30px;
}

.song-name {
  font-weight: bold;
}
.artist-name {
  color: #D90053;
}
</style>
