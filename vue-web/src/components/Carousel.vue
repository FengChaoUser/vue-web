<template>
  <div>aaa</div>
</template>

<script>
// 定义每个小图标的宽度
const iconWidth = 30;
export default {
  name: 'Carousel',
  /*
  * 接收父组件的值,轮播图宽度、高度、相邻两张图片切换的间隔时间、轮播图路径数组
  */

  data() {
  	return {
      // 子组件中模拟的轮播图数组，实际开发中应由父组件传入

  	}
  },
  created() {

  },
  computed: {

  },
  beforeDestroy() {
    // 组件销毁前,清除监听器

  },
  methods: {

  },
  components: {

  }
};
</script>

<style scoped>

	@import '../assets/css/carousel.css';

</style>
