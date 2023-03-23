<template>
  <TimerVue />
</template>

<script>
import TimerVue from "./components/Timer.vue";

export default {
  name: "App",
  components: {
    TimerVue,
  },
};
</script>

<style>
#app {
  font-family: gill sans, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
