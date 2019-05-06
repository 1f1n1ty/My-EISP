<template>
  <div>
    <div class="search">
      <div class="inputBox">
        <input  type="text" placeholder="搜索">
      </div>
    </div>
    <!-- 轮播图 -->
    <swiper class="banner" circular autoplay indicator-dots indicator-active-color="rgba(255,255,255,0.8)" indicator-color='rgba(255,255,255,0.7)'>
      <swiper-item v-for="(list,key) in bannerList" :key="key">
        <navigator :url="list.navigator_url">
          <image :src="list.image_src"></image>
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 导航 -->
    <div class="nav">
      <navigator :url="list.navigator_url" v-for="(list,key) in navList" :key="key">
        <image :src="list.image_src"></image>
      </navigator>
    </div>
    <!-- 商品楼层 -->
    <div class="floors">
      <div class="floor" v-for="(list,key) in floorList" :key="key">
        <div class="title">
          <navigator>
            <image :src="list.floor_title.image_src"></image>
          </navigator>
        </div>
        <div class="pics">
          <navigator :url="item.navigator_url" v-for="(item,index) in list.product_list" :key="index">
            <image :src="item.image_src"></image>
          </navigator>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import request from '../../utils/request.js'

export default {
  data () {
    return {
      bannerList:[],
      navList:[],
      floorList:[],
    }
  },
  methods: {
    // 轮播图
    async getBanner () {
      let {message} = await request({
        url:'/api/public/v1/home/swiperdata'
      })
      // console.log(message)
      this.bannerList = message
    },
    // 导航
    async getNav () {
      let {message} = await request({
        url:'/api/public/v1/home/catitems'
      })
      // console.log(message)
      this.navList = message
    },
    // 楼层
    async getFloor () {
      let {message} = await request({
        url:'/api/public/v1/home/floordata'
      })
      console.log(message)
      this.floorList = message
    }
    
  },

  mounted () {
    this.getBanner()
    this.getNav()
    this.getFloor()
  },

  onPullDownRefresh () {
    this.getBanner()
    this.getNav()
    this.getFloor()
    mpvue.stopPullDownRefresh()
  }
}
</script>

<style scoped>
  .search {
    background-color:#ff2d4a;
  }
  .search .inputBox {
    padding: 20rpx 30rpx;
  }
  .search .inputBox input{
    height: 65rpx;
    background-color: #FFF;
    padding-left: 15rpx;
    border-radius: 8rpx;
    font-size: 27rpx;
    color: #666;
  }
  .banner {
    width: 750rpx;
    height: 340rpx;
  }
  .banner navigator{
    height: 100%;
    width:100%;
  }
  .nav {
    display: flex;
  }
  .nav navigator {
    height: 140rpx;
    width: 128rpx;
    padding:30rpx;
    flex: 1;
  }
  .floors .floor .title {
    width: 750rpx;
    height: 60rpx;
    padding-top: 27rpx;
    padding-left: 15rpx;
    background-color: #f4f4f4;
  }
  .floors .floor .title image {
    height: 59rpx;
  }
  .floors .pics {
    padding: 20rpx 18rpx;
    overflow: hidden;
  }
  .floors .pics navigator{
     height: 188rpx;
     margin-left: 10rpx;
     margin-bottom: 10rpx;
     float: left;
  }

  .floors .pics navigator:nth-child(1){
    width: 232rpx;
    height: 386rpx;
    margin-left: 0;
  }
  .floors .pics navigator:nth-child(2){
    width: 273rpx;
  }
  .floors .pics navigator:nth-child(3){
    width: 193rpx;
  }
  .floors .pics navigator:nth-child(4){
    width: 193rpx;
    margin-bottom: 0rpx;
  }
  .floors .pics navigator:nth-child(5){
    width: 273rpx;
    margin-bottom: 0rpx;
  }

</style>
