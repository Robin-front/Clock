<template>
    <svg :style="{transform:`rotate(${rotate}deg)`}" width="200px" height="200px" viewBox="0 0 200 200">
      <circle
        v-for="(item, i) in circleArray"
        :key="i"
        r="50"
        cx="100"
        cy="100"
        fill="none"
        stroke-width="100"
        :stroke="item.stroke"
        :stroke-dasharray="`${item.strokeDasharray} 314`"
        :stroke-dashoffset="item.dashoffset"
      />
    </svg>
</template>

<script>
export default {
  name: 'Clock',
  data() {
    return {
      rotate: -90,
      lastTimeStamp: performance.now(),
      color: ['yellowgreen', '#CC6600', '#FF0000'],
      circleArray: Array.from({length: 4}, () => ({
        stroke: 'yellowgreen',
        strokeDasharray:1,
        dashoffset: -1
      })),
      frameTime: 1000 / 60,
    }
  },
  mounted() {
    requestAnimationFrame(this.walker);
  },
  methods: {
    walker() {
      const frameLength = Math.round((performance.now() - this.lastTimeStamp)/this.frameTime);
      this.lastTimeStamp = performance.now();
      let level = 0;
      if (frameLength <= 3) {
        level = 0;
      } else if (frameLength <= 6) {
        level = 1;
      } else {
        level = 2;
      }
      const strokeDasharray = 1 * frameLength;
      
      this.circleArray.unshift({
        stroke: this.color[level],
        strokeDasharray,
        dashoffset: 0
      });
      if (this.circleArray.length > 4) {
        this.circleArray.pop();
      }
      const width = this.circleArray.reduce((sum, circle) => {
        circle.dashoffset = -sum+1;
        sum += circle.strokeDasharray;
        return sum;
      }, 0);
      this.rotate = (-width)/314*360-90;
      requestAnimationFrame(this.walker);
    }
  }
}
</script>

<style>
@keyframes clock {
  to {
    transform: rotate(360deg);
  }
}

svg {
  transform: rotate(-90deg);
  background: black;
  border-radius: 50%;
}

circle {
  animation: clock 6s linear infinite;
  transform-origin: 100px 100px;
}
</style>
