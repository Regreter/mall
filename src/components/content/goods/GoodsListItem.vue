<template>
  <div class="goods-item" @click="itemClick">
    <img :src="showImage" alt="" @load="imageLoad">
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">{{"  "+"☆"}}{{goodsItem.cfav}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GoodsListItem",
    props: {
      goodsItem: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    computed: {
      showImage() {
        return this.goodsItem.image || this.goodsItem.show.img
      }
    },
    methods: {
      imageLoad() {
        this.$bus.$emit('homeItemImgLoad')
        if (this.$route.path.indexOf('/home')) {
          this.$bus.$emit('itemImageLoad')
        } else if (this.$route.path.indexOf('./detail')) {
          this.$bus.$emit('detailItemImgLoad')
        }
      },
      itemClick() {
        this.$router.push('/detail/' + this.goodsItem.iid)
      }
    }
  }
</script>

<style  scoped>
  .goods-item {
    padding-bottom: 40px;
    position: relative;

    width: 48%;
  }

  .goods-item img {
    width: 100%;
    border-radius: 5px;
  } 

  .goods-info {
    font-size: 12px;
    position: absolute;
    bottom: 5px;
    left: 0;
    right: 0;
    overflow: hidden; /* 溢出隐藏 */
    text-align: center;
  }

  .goods-info p {
    overflow: hidden;
    text-overflow: ellipsis; /* 显示省略符号来代表被修剪的文本 */
    white-space: nowrap; /* 规定文本不会换行，直到遇到 <br> 标签为止 */
  }
  
  .goods-info .price {
    position: relative;
    color: red;
  }

  .goods-info .collect::before {
    content: '';
    position: absolute;
    left: -15px;
    top: -1px;
    width: 14px;
    height: 14px;
  }
</style>
