<template>
  <div id="app">
    <AudioMixer />
  </div>
</template>

<script>
import AudioMixer from './components/AudioMixer.vue'

export default {
  name: 'App',
  components: {
    AudioMixer
  }
}
</script>

<style>
html, body {
  background-color:#1e2328;
  font-family: Helvetica;
}
</style>
