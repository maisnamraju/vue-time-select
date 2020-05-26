<template>
  <li @click="selected(time)">{{ time }}</li>
</template>

<script>
export default {
  props: ["date", "format"],
  computed: {
    time() {
      let returnVal = null;
      // probably move the case results into their own functions
      switch (this.format) {
        case "hh:mm:ss":
          returnVal = `${this.padding(this.date.getHours())}:${this.padding(
            this.date.getMinutes()
          )}:${this.padding(this.date.getSeconds())}`;
          break;
        case "hh:mm A":
          returnVal = this.getAMPM(this.date);
          break;
        default:
          returnVal = this.getAMPM(this.date);
          break;
      }
      return returnVal;
    }
  },
  methods: {
    selected() {
      this.$emit("selected", { d: this.date, t: this.time });
    },
    padding(item) {
      return item < 10 ? `0${item}` : item;
    },
    getAMPM(date) {
      let AMPM = date.getHours() > 11 ? "PM" : "AM";
      let hour = date.getHours() > 11 ? date.getHours() - 12 : date.getHours();
      return `${this.padding(hour)}:${this.padding(date.getMinutes())} ${AMPM}`;
    }
  }
};
</script>
