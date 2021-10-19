<template>
  <div id="app">
    <ColorGenerator />
  </div>
</template>

<script>
import ColorGenerator from "./components/ColorGenerator.vue";

export default {
  components: {
    ColorGenerator,
  },
};
</script>

<style>
#app {
  font-family: "Quicksand", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 50px;
}
</style>
