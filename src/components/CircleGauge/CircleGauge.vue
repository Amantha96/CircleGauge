<!-- src/components/CircleGauge.vue -->
<template>
    <div class="circle-gauge">
      <svg :width="size" :height="size" viewBox="0 0 100 100">
        <circle class="bg-circle" cx="50" cy="50" r="45"></circle>
        <circle
          v-for="(interval, index) in intervals"
          :key="index"
          :class="'interval-circle interval-' + index"
          :cx="50"
          :cy="50"
          :r="45"
          :stroke="interval.color"
          :stroke-dasharray="getDashArray(interval.value)"
          :transform="getRotation(index)"
        ></circle>
      </svg>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  interface IntervalItem {
    value: number;
    color: string;
  }
  
  export default defineComponent({
    props: {
      intervals: {
        type: Array as () => IntervalItem[],
        required: true,
      },
      size: {
        type: Number,
        default: 100,
      },
    },
    methods: {
      getDashArray(value: number): string {
        return `${value}, 100`;
      },
      getRotation(index: number): string {
        const total = this.intervals.reduce((acc, interval, i) => (i < index ? acc + interval.value : acc), 0);
        return `rotate(${(total / 100) * 360} 50 50)`;
      },
    },
  });
  </script>
  
  <style scoped>
  .circle-gauge {
    display: inline-block;
  }
  
  .bg-circle {
    fill: none;
    stroke: #ccc;
    stroke-width: 10;
  }
  
  .interval-circle {
    fill: none;
    stroke-width: 10;
    stroke-linecap: round;
    transform-origin: 50% 50%;
    transform: rotate(0 50 50);
  }
  
  .interval-0 {
    stroke: #E1CA46;
  }
  
  .interval-1 {
    stroke: #75C952;
  }
  
  .interval-2 {
    stroke: #469ED5;
  }
  </style>
  