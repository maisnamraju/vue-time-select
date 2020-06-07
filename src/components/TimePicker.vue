<template>
  <div class="time-picker">
    <button @click="show = !show">
      <slot>
        <span v-if="value.t">{{ value.t }} </span>
        <span v-else>{{ placeholder }} </span>
        <svg-icon class="h-5 w-5"></svg-icon>
      </slot>
    </button>
    <div class="time-list-container" v-show="show">
      <ul class="time-list">
        <time-option
          v-for="(time, i) in timeArr"
          :key="i"
          :date="time"
          :format="format"
          @selected="selected"
          @removed="removed($event, i)"
        ></time-option>
      </ul>
    </div>
  </div>
</template>

<script>

import TimeOption from "@/components/TimeOption";
import svgIcon from '@/components/svg';
export default {
  props: {
    value: {
      type:Object,
      default(){
        return {

        }
      }
    },
    placeholder: {
      type: String,
      default: "Select a Time",
    },
    format: {
      type: String,
      default: "hh:mm A", // will only support hh:mm:ss and hh:mm A
    },
    from: {
      type: String,
      default: "00:00:00",
    },
    to: {
      type: String,
      default: "23:59:59",
    },
    interval: {
      type: Number,
      default: 15,
      validator(val) {
        return val >= 1 && val <= 60;
      },
    },
    date: {
      // create a validator fro this later
      type: Date,
      default() {
        return new Date();
      },
    },
    multi: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    TimeOption,
    svgIcon
  },
  data: () => ({
    selectedArr: [],
    timeArr: [],
    show: false
  }),
  mounted() {
    //this.$nextTick(() => {
    this.initialize();
    //});
  },
  methods: {
    initialize() {
      let startTime = new Date(this.date.getTime());
      let endTime = new Date(this.date.getTime());

      startTime.setHours(...this.getTimeArray(this.from));
      endTime.setHours(...this.getTimeArray(this.to));
      this.generateOptions(startTime, endTime, this.interval);
    },
    getTimeArray(timeStr = "00:00:00") {
      let arr = timeStr.split(":").map((i) => +i);
      return arr;
    },
    generateOptions(startTime, endTime, interval) {
      let arr = [];
      let time = new Date(startTime.getTime());
      while (time < endTime) {
        if (endTime > new Date(time.getTime() + interval * 60 * 1000)) {
          arr.push(new Date(time.getTime() + interval * 60 * 1000));
          time = new Date(time.getTime() + interval * 60 * 1000);
        } else {
          break;
        }
      }
      this.timeArr = [startTime, ...arr];
    },
    selected(val) {
      this.$emit("input", val);
      this.show = false;
      if (this.multi == true) {
        this.selectedArr.push(val);
        this.$emit("multi-select-updated", this.selectedArr);
      }
    },
    removed(e, i) {
      this.$removed("removed", e);
      if (this.multi == true) {
        this.selectedArr = this.selectedArr.splice(i, 1);
        this.$emit("multi-select-updated", this.selectedArr);
        this.$emit("multi-select-removed", { ...e, index: i });
      }
    },
  },
};
</script>
<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>
<style lang="scss">
.time-picker {
  @apply .flex .flex-col .items-center .relative;
  width: var(--width);
  background: var(--button-color);
  border-color: var(--border-color);
  &:hover, &:focus{
    @apply .outline-none;
    border-color: var(--hover-border-color);
  }
  button {
    @apply .inline-flex .justify-between .items-center .outline-none .w-full .border .rounded .px-2 .py-1;
    width: var(--width);
  }
  
  &>div {
    @apply .text-center .my-1;
    width: var(--width);
    &:first-child {
      @apply .z-20;
    }
  }
  .time-list-container {
    @apply .absolute .shadow .z-10 .w-full .rounded .overflow-y-scroll;
    background-color: var(--list-bg-color);
    top: 100%;
    height: var(--height-time-options, 150px);
    .time-list {
      li {
        @apply .cursor-pointer .w-full .border-gray-100;
        &:hover { 
          color: var(--hover-list-text-color);
          background: var(--hover-list-bg-color);
          }
      }
    }
  }
}
</style>
