<template>
  <div class="nav-bar">
    <div class="left"><slot name="left"></slot></div>
    <div class="center"><slot name="center"></slot></div>
    <div class="right"><slot name="right"></slot></div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
      }
    }
  }
</script>

<style  scoped>
  .nav-bar {
    display: flex;
    line-height: 44px; /* 导航栏高度一般都是44px,有内容才会撑起来 */
    text-align: center;
    box-shadow: 0 1px 1px rgba(100, 100, 100, .1); /* 设置阴影 */
  }

  .left, .right {
    width: 60px;
  }

  .center {
    flex: 1; /* 占据其余剩余部分 */
  }
</style>
