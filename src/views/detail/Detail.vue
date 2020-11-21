<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav"/>
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages"/>
      <detail-base-info :goods="goods"/>
      <detail-shop-info :shop="shop"/>
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad"/>
      <detail-param-info :param-info="paramInfo"/>
      <detail-comment-info :comment-info="commentInfo"/>
      <goods-list :goods="recommends"/>
    </scroll>
  </div>
</template>

<script>
  import DetailNavBar from './childComps/DetailNavBar'
  import DetailSwiper from './childComps/DetailSwiper'
  import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
  import DetailShopInfo from './childComps/DetailShopInfo.vue'
  import DetailGoodsInfo from './childComps/DetailGoodsInfo.vue'
  import DetailParamInfo from './childComps/DetailParamInfo.vue'
  import DetailCommentInfo from './childComps/DetailCommentInfo.vue'

  import Scroll from '../../components/common/scroll/Scroll'
  import GoodsList from '../../components/content/goods/GoodsList.vue'

  import {getDetail, Goods, Shop, GoodsParam, getRecommend} from "@/network/detail"


  export default {
    name: 'Detail',
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      Scroll,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      GoodsList,
    },
    data () {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: []
      }
    },
    created() {
      // 1.保存传入的iid
      this.iid = this.$route.params.iid

      // 2.根据iid请求详情页数据
      getDetail(this.iid).then(res => {
        // console.log(res);
        // 1).获取数据
        const data = res.result
        // 2).获取顶部的图片轮播数据
        this.topImages = data.itemInfo.topImages
        // 3).获取商品信息
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        // 4).获取店铺信息的对象
        this.shop = new Shop(data.shopInfo)
        // 5).保存商品详情数据
        this.detailInfo = data.detailInfo
        // 6).获取参数信息
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
        // 7).取出评论信息
        if (data.rate.cRate != 0) {
          this.commentInfo = data.rate.list[0]
        }
      })

      // 3.请求推荐数据
      getRecommend().then(res => {
        console.log(res);
        this.recommends = res.data.list
      })
    },
    // mounted() {
    //   const refresh = this.debounce(this.$refs.scroll.refresh, 200)
    //   this.$bus.$on('itemImageLoad', () => {
    //     refresh()
    //   })
    // },
    methods: {
      // 防抖操作
      // debounce(func, delay) {
      //   let timer = null
      //   return function (...args) {
      //     if (timer) clearTimeout(timer)
      //     timer = setTimeout(() => {
      //       func.apply(this, args)
      //     }, delay)
      //   }
      // },
      imageLoad() {
        this.$refs.scroll.refresh()
      }
    }
  }
</script>

<style  scoped>
  #detail {
    position: relative;
    z-index: 10;
    background-color: #fff;
    height: 100vh; /* 设置100%的视图高度 */
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px);
    /* overflow: hidden; */
  }

  
</style>
