<template>
  <div class="circle">
    <div
      :class="['line', isActive(item)]"
      v-for="item in 180"
      :key="item"
      :style="{ transform: `rotate(${item*2}deg)` }"
    ></div>
  </div>
</template>

<script>
export default {
  name: "Clock",
  data() {
    return {
      lastTimeStamp: Date.now(),
      active: [{ from: 0, to: 0, level: "green" }],
    };
  },
  mounted() {
    requestAnimationFrame(this.walker);
  },
  methods: {
    walker() {
      requestAnimationFrame(this.walker);
      const time = (Date.now() - this.lastTimeStamp) / 20;
      this.lastTimeStamp = Date.now();
      const item = this.active[this.active.length - 1];
      if (time > 3) {
        this.active.push({
          from: item.to,
          to: (item.to + time) % 180,
          level: "red",
        });
      } else {
        this.active.push({
          from: item.to,
          to: (item.to + time) % 180,
          level: "green",
        });
      }
      if (this.active.length > 2) {
        this.active.shift();
      }
    },
    isActive(cur) {
      let className = "";
      this.active.some((item) => {
        if (
          (cur > item.from && cur < item.to) ||
          (item.to < item.from && (cur > item.from || cur < item.to))
        ) {
          className = item.level;
          return true;
        }
        return false;
      });
      return className;
    },
  },
};
</script>

<style>
circle {
  animation: clock 6s linear infinite;
  transform-origin: 100px 100px;
}
.circle {
  position: relative;
  width: 300px;
  height: 300px;
  border-radius: 50%;
  background-color: black;
}
.line {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 50%;
  height: 6px;
  border-radius: 4px;
  transform-origin: 0 3px;
}
.green {
  background-color: chartreuse;
}
.red {
  background-color: red;
}
</style>
