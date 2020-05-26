<template>
  <div class="time-picker">
    <div>
      {{ placeholder }}
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

export default {
  props: {
    placeholder: {
      type:String,
      default: 'Select a Time'
    },
    format: {
      type: String,
      default: "hh:mm A" // will only support hh:mm:ss and hh:mm A
    },
    from: {
      type: String,
      default: "00:00:00"
    },
    to: {
      type: String,
      default: "23:59:59"
    },
    interval: {
      type: Number,
      default: 15,
      validator(val) {
        return val >= 1 && val <= 60;
      }
    },
    date: {
      // create a validator fro this later
      type: Date,
      default() {
        return new Date();
      }
    },
    multi: {
      type: Boolean,
      default: false
    }
  },
  components: {
    TimeOption
  },
  data: () => ({
    selectedArr: [],
    timeArr: []
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
      let arr = timeStr.split(":").map(i => +i);
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
      this.$emit("selected", val);
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
    }
  }
};
</script>

<style lang="scss">
  .time-list {

  }
</style>
