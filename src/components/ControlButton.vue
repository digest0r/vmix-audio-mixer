<template>
  <button
    :class="{
      'control-button-controls-button': true,
      'control-button-controls-button--active': active,
    }"
    @click="toggleButton()"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: "AudioInput",
  props: {
  },
  data() {
    return {
      active: false,
    };
  },
  methods: {
    toggleButton() {
      this.active = !this.active;
    }
  }
};
</script>

<style scoped>
.control-button-controls-button {
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
.control-button-controls-button.control-button-controls-button--active {
  background-color: #006400;
  border-color: #189618;
}
</style>
