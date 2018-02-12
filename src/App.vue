<template>
  <div id="app">
    <tab
      :list = 'tablist'
      class= "tab"
      :offset = 'offset'
      :newIndex = 'currentIndex'
      @indexChange = 'tabClicked'
    ></tab>
    <div class="currentShow">
      index: {{currentShow}}
    </div>
    <div class="btns">
      <button @click="currentIndex = 0">go to 0</button>
      <button @click="currentIndex = 5">go to 5</button>
    </div>
  </div>
</template>

<script>
import TWEEN from '@Tweenjs/tween.js'
import Tab from './tab'
export default {
  name: 'app',
  components: {
    Tab
  },
  data () {
    return {
      tablist: ['相声', '小品', '魔术', '杂技', '评书', '笑话', '曲苑杂坛', '哎嘿', '曲苑~', '杂坛'],
      currentIndex: 0,
      currentShow: 0,
    }
  },
  methods: {
    tabClicked(newValue) {
      this.currentIndex = newValue;
    }
  },
  computed: {
    offset() {
      return 175;
    }
  },
  watch: {
    currentIndex(now, old) {
      if(TWEEN.update()){
        requestAnimationFrame(animate);
      }
      let vm = this;
      function animate(time) {
        requestAnimationFrame(animate);
        TWEEN.update(time);
      }
      let tweening = { tweeningNumber: old}
      new TWEEN.Tween(tweening)
          .easing(TWEEN.Easing.Quadratic.Out)
          .to({ tweeningNumber: now }, 500)
          .onUpdate(function () {
            vm.currentShow = tweening.tweeningNumber.toFixed(0);
          })
          .start()
      animate();
    }
  },
  created () {
    console.log(window.innerWidth);
    console.log(window.document.documentElement.clientWidth);
  }
}
</script>

<style lang="less">
html, body{
  margin: 0;
  padding: 0;
  #app>.tab{
    font-size: 20px;
    >ul>li.tab-item{
      padding: 0 15px;
    }
  }
  .currentShow{
    text-align: center;
    height: 250px;
    line-height: 250px;
    font-size: 50px;
  }
  .btns{
    button{
      display: block;
      width: 90%;
      background-color: #f8f8f8;
      border: 1px solid #ccc;
      outline: none;
      margin: 10px auto;
      padding: 10px;
      color: #353535;
      &:active{
        background-color: #f0f0f0;
      }
    }
  }
}
</style>

