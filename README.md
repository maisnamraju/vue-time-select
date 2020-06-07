# Vue Time Picker Select 

![GitHub version](https://img.shields.io/github/package-json/v/maisnamraju/vue-time-select?color=success&style=flat-square)
![npm version](https://img.shields.io/npm/v/vue-time-select?style=flat-square)
![GitHub release](https://img.shields.io/github/release/maisnamraju/vue-time-select?label=github&style=flat-square)
![npm downloads](https://img.shields.io/npm/dm/vue-time-select?style=flat-square)

A jQuery Timepicker inspired time picker for vuejs using a select style dropdown. Current Supports `AM/PM` format and `HH:mm:ss` formats, more in the pipeline, supports styling using css variables. 
No external dependencies other than tailwindcss. 

### Installation

```bash
npm i -S vue-time-picker-select

```
```bash
yarn add vue-timepicker-select
```

### Using the timepicker

Import the timepicker into your component 
```javascript
 import VueTimePickerSelect from 'vue-timepicker-select`;

 export default {
    components: {
        VueTimePickerSelect
    },
    data:() => ({
        date: new Date(),
        placeholder: 'Select A Time',
        timeVal: {},
    }),
 }
 
 <vue-timepicker-select 
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
```

Currently the timepicker supports only 2 formats `AM\PM` and `24 hours`. Use 
`hh:mm A` for the AMPM format and `hh:mm:ss` for the 24 hour format 


## This is still a work in progress and more features are on the way. Feel free to create an issue or if you need any features. Will try and update the features based on my availablity.

