<template>
  <div id="app">
    <Time-Picker :date="date" from="12:20:00" format="hh:mm:ss" />
  </div>
</template>

<script>
import TimePicker from "@/components/TimePicker";

export default {
  name: "App",
  components: {
    TimePicker
  },
  data:() => ({
    date: new Date()
  })
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
