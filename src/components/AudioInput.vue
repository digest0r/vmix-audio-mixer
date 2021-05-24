<template>
  <div class="audio-input">
    <div class="audio-input-header">
      <div class="audio-input-number">{{ audioInput.number }}</div>
      <div class="audio-input-name">{{ audioInput.shortTitle }}</div>
    </div>

    <div class="audio-input-body">
      <div class="audio-input-controls">
        <ControlButton>S</ControlButton>
        <ControlButton>&lt;&gt;</ControlButton>
        <ControlButton>A</ControlButton>
      </div>
      <div class="audio-input-volume">
        <input
          orient="vertical"
          type="range"
          min="1"
          max="100"
          @input="(event) => debouncedOnSetVolume(event.target.value)"
        />
      </div>
    </div>

    <div class="audio-input-footer">
      <div class="audio-input-footer-controls">
        <ControlButton>M</ControlButton>
        <ControlButton>A</ControlButton>
        <ControlButton>B</ControlButton>
      </div>
    </div>
  </div>
</template>

<script>
import debounce from "lodash.debounce";
import ControlButton from "./ControlButton";

export default {
  name: "AudioInput",
  components: {
    ControlButton,
  },
  props: ["audioInput", "onSetVolume"],
  data() {
    return {
      debouncedOnSetVolume: () => ({}),
    };
  },
  mounted() {
    this.debouncedOnSetVolume = debounce(this.onSetVolume, 150);
  }
};
</script>

<style scoped>
.audio-input {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 300px;
  width: 140px;
  margin: 0 8px;
  background-color: #0a0f14;
  color: #d1d1d1;
}
.audio-input-header {
  display: flex;
  font-family: Helvetica;
  border-bottom: 1px solid #181818;
}
.audio-input-number {
  padding: 0.1rem 0.5rem;
  font-weight: bold;
  background-color: #181818;
}
.audio-input-name {
  padding: 0.1rem 0.4rem;
  white-space: nowrap;
  overflow: hidden;
}
.audio-input-body {
  display: flex;
}
.audio-input-controls {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 33.33333%;
}
.audio-input-controls-button {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 26px;
  height: 26px;
  border: 1px solid #70767c;
  background-color: #293038;
  margin: 2px;
  padding: 2px;
  color: white;
  cursor: pointer;
}
.audio-input-controls-button.audio-input-controls-button--active {
  background-color: #006400;
  border-color: #189618;
}
.audio-input-volume {
  display: flex;
  justify-content: center;
  width: 66.66666%;
  -webkit-appearance: slider-vertical;
}
.audio-input-volume input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  width: 3px;
  height: 100%;
  background: #405656;
  outline: none; /* Remove outline */
  opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
  -webkit-transition: 0.2s; /* 0.2 seconds transition on hover */
  transition: opacity 0.2s;
}
.audio-input-volume input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 10px;
  background: #405656;
  border: 1px solid white;
  border-radius: unset;
  cursor: pointer;
}
.audio-input-volume input::-moz-range-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 25px;
  height: 10px;
  background: #405656;
  border: 1px solid white;
  border-radius: unset;
  cursor: pointer;
}
.audio-input-footer-controls {
  display: flex;
}
</style>
