<template>
  <div id="tab-bar">
    <slot></slot>
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
  #tab-bar {
    display: flex;/* 水平 */
    background-color: #f6f6f6;

    position: fixed;/* 固定定位 */
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 9;
 
    box-shadow: 0 -1px 1px rgba(100,100,100,0.2);/* 设置阴影 */
  }
</style>
