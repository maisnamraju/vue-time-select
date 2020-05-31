<template>
  <div id="app">
    <Hello-World>
      <div class="flex-auto flex flex-col items-center h-24 mt-20">
      <Time-Picker 
      :date="date" 
      from="12:20:00" 
      format="hh:mm A"
      v-model="timeVal"
      :style="{
        '--button-color': '#cbd5e0',
        '--border-color': '#4DA2DD',
        '--hover-border-color': '#007ace',
        '--hover-list-text-color': '#f5f5f5',
        '--hover-list-bg-color': '#5c6ac4',
        '--list-bg-color': '#DDDDDD',
        '--width': 'auto'
      }"
      :placeholder.sync="placeholder" />
      <code>
        {{ code }}
      </code>
      </div>
      <div class="flex-auto flex flex-col items-center h-24 mt-2">
        <Time-Picker :date="date" from="11:20:00" to="22:20:30" :interval="30" format="hh:mm:ss" :placeholder.sync="placeholder" />
      </div>
    </Hello-World>
  </div>
</template>

<script>
import TimePicker from "@/components/TimePicker";
import HelloWorld from "@/components/HelloWorld";
export default {
  name: "App",
  components: {
    TimePicker,
    HelloWorld
  },
  data: () => ({
    date: new Date(),
    placeholder: 'Select A Time',
    timeVal: {},
    code: `<Time-Picker :date="date" from="12:20:00" format="hh:mm A" :placeholder.sync="placeholder" />`
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
