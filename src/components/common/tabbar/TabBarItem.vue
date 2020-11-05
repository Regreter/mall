<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-icon-active"></slot></div>
    <div :style="activeStyle"><slot name="item-text"></slot></div>
  </div>
</template>

<script>
  export default {
    props: {
      path: String,
      activeColor: {
        type: String,
        default: 'purple'
      }
    },
    data () {
      return {
        // isActive: true,
      }
    },
    computed: {
      isActive() {
        // 判断处于活跃路由的path包不包含该文件传入的path,
        // == -1 表示this.$route.path里没有找到this.path,找到就是 !== -1
        return this.$route.path.indexOf(this.path) !== -1
      },
      activeStyle() {
        // 如果处于活跃状态则动态绑定传入的颜色,否则给个空对象
        return this.isActive ? {color: this.activeColor} : {}
      }
    },
    methods: {
      itemClick() {
        this.$router.push(this.path)
      }
    }
  }
</script>

<style  scoped>
  .tab-bar-item {
    flex: 1;
    text-align: center;
    height: 49px;
    font-size: 14px;
  }

  .tab-bar-item img {
    height: 24px;
    width: 24px;
    margin-top: 3px;
    vertical-align: middle; /* 去掉图片下面默认的3个像素 */
    margin-bottom: 2px;/* 图片底部增加2个像素 */
  }

  /* .active {
    color: purple;
  } */
</style>
