<template>
  <div class="half-circle-gauge" @click="handleClick">
    <svg class="svg" :width="size" :height="size / 1.2">
      <defs>
        <linearGradient id="gradient" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop v-for="(interval, index) in intervals" :key="index" :offset="(index * 100 / (intervals.length - 1)) + '%'"
            :style="'stop-color:' + interval.color" />
        </linearGradient>
      </defs>
      <circle v-for="(interval, index) in intervals" :key="index" :class="'interval-circle interval-' + index" :cx="50"
        :cy="50" :r="40" :stroke-dasharray="computedDashArray(index)"></circle>
      <text :x="50" :y="50" class="speed-text" text-anchor="middle" dominant-baseline="middle">{{ currentSpeed }}</text>
    </svg>

    <div class="speed-display" v-if="showSpeed">
      {{ speed }}%
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

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
  data() {
    return {
      showSpeed: false,
      speed: 0,
      currentSpeed: 0,
    };
  },
  methods: {
    handleClick(event: MouseEvent) {

      const boundingBox = this.$el.getBoundingClientRect();
      const centerX = boundingBox.left + boundingBox.width / 2;
      const centerY = boundingBox.top + boundingBox.height / 2;
      const clickX = event.clientX;
      const clickY = event.clientY;
      const angleRad = Math.atan2(clickY - centerY, clickX - centerX);
      const angleDeg = (angleRad * 180) / Math.PI;
      const normalizedAngle = (angleDeg + 360) % 360;


      const intervalIndex = Math.floor((normalizedAngle / 360) * (this.intervals.length - 1));
      const interval = this.intervals[intervalIndex];
      this.speed = parseInt((interval.value).toFixed(0), 10);

      this.showSpeed = true;


      setTimeout(() => {
        this.showSpeed = false;
      }, 2000);
    },
    computedDashArray(index: number) {

      const angleRad = (index * 360) / (this.intervals.length - 1) * (Math.PI / 180);


      const dashLength = (2 * Math.PI * 40 * angleRad) / (2 * Math.PI);


      const gapLength = 251.2 - dashLength;

      return `${dashLength} ${gapLength}`;
    },

  },
});
</script>

<style scoped>
.half-circle-gauge {
  display: flex;
  align-items: center;
  justify-content: center;
}

.interval-circle {
  fill: none;
  stroke-width: 15;
  stroke-linecap: round;
  transform-origin: 50% 50%;
  transform: rotate(0 50 50);
}

.interval-0 {
  stroke: url(#gradient);
}

.speed-display {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 24px;
  font-weight: bold;
  color: #333;
}
</style>
