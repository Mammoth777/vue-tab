<template>
  <div class="tab">
    <ul ref="tabul">
      <li v-for="(item, index) in list" :key="index" @click="current = index" v-getWidth ref="lis">
        {{item}}
      </li>
      <li class="line" :style='lineStyle'>
      </li>
    </ul>
  </div>
</template>

<script>
import TWEEN from "@Tweenjs/tween.js";
import { clearTimeout } from "timers";
export default {
  name: "tabComponent",
  data() {
    return {
      current: 0,
      tempPos: -1,
      offset: 130, // 当前 tab 距离屏幕左边的偏移量
      lineStyle: {}
    };
  },
  props: {
    list: {
      type: Array
    },
    newIndex: {
      type: Number,
      default: 0
    }
  },
  directives: {
    getWidth: {
      inserted(el) {
        el.setAttribute("data-width", el.offsetWidth); // 获取自身宽度并设置`data-width`
      }
    }
  },
  methods: {
    getSum(index) {
      // 获取当前位置相对最左侧的偏移
      let lis = this.$refs.lis;
      let result = 0;
      for (let i = 0; i < index; i++) {
        result += lis[i].dataset.width * 1;
      }
      return result;
    },
    setLineStyle() {
      const lis = this.$refs.lis;
      const currentPos = this.getSum(this.current);
      let left = "";
      this.$nextTick(() => {
        /* if (this.$refs.tabul.scrollLeft === 0) {
          left = currentPos + "px";
          console.log({ currentPos, sleft: this.$refs.tabul.scrollLeft });
        } else {
          left = this.offset + "px";
        } */
        this.lineStyle = {
          width: lis ? lis[this.current].offsetWidth + "px" : 0,
          left: currentPos + "px"
        };
      });
    }
  },
  watch: {
    current(now, old) {
      this.$emit("indexChange", now); // 向父元素报告新 index
      this.setLineStyle();
      // 通过检测current的改变, 来自动完成 tab 部分的滚动效果
      let endpos = this.getSum(now) - this.offset;
      let ele = this.$refs.tabul;
      let vm = this;
      // Setup the animation loop.
      function animate(time) {
        requestAnimationFrame(animate);
        TWEEN.update(time);
      }
      requestAnimationFrame(animate);
      const coords = { x: ele.scrollLeft };
      let tween = new TWEEN.Tween(coords)
        .to({ x: endpos }, 500)
        .easing(TWEEN.Easing.Quadratic.Out)
        .onStart(() => {
          // console.log(now);
        })
        .onUpdate(function() {
          ele.scrollLeft = coords.x;
        })
        .onComplete(() => {
          vm.setLineStyle();
        })
        .start();
    },
    newIndex(now, old) {
      this.current = now;
    }
  },

  // 生命周期
  mounted() {
    this.setLineStyle();
  }
};
</script>

<style lang="less" scoped>
.tab {
  > ul {
    display: flex;
    overflow-x: scroll;
    position: relative;
    > li {
      padding: 0 10px;
    }
    > li.line {
      position: absolute;
      left: 0;
      bottom: 0;
      height: 2px;
      background-color: #353535;
      transition: all 0.3s ease-in-out;
      width: 0px;
      padding: 0;
    }
  }
}
</style>
