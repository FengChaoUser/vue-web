<template>
  <div id="box">
    <!-- 控制数据的值切换显示隐藏 -->
    <button @click="show=!show">transition</button>
    <transition name="fade">
      <p v-if="show"></p>
    </transition>
  </div>
</template>

<script>
  export default {
    name: "transition2",
    data() {
      return {
        show: false
      }
    }
  }
</script>

<style scoped>
  p {
    width: 300px;
    height: 300px;
    background: red;
  }

  /* 定义动画 */


  /*
  * 定义过渡
  */

  /* .fade-enter,.fade-leave-active {
      opacity: 0
  } */

  /*
  * 定义宽高动画
  */
  .fade-enter-active, .fade-leave-active {
    transition: all 1s ease;
  }
  .fade-enter-active {
    opacity: 1;
  }

  .fade-leave-active {
    opacity: 0;
  }

  /* 重要：定义初始状态 */
  .fade-enter {
    opacity: 0;
  }
</style>
