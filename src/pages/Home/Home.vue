<template>
    <section class="home">
      <!--首页头部-->
      <v-HeaderTop :title="address.name">
        <router-link class="header_search" slot="left" to="/search">
          <i class="iconfont icon-sousuo"></i>
        </router-link>

        <router-link class="header_login" slot="right" :to="userInfo._id ? '/userInfo' : '/login'">
          <span class="header_login_text" v-if="!userInfo._id">
          登录|注册
        </span>
          <span class="header_login_text" v-else>
           <i class="iconfont icon-person"></i>
        </span>
        </router-link>
      </v-HeaderTop>

      <!--首页导航-->
      <nav class="home_nav">
        <div class="swiper-container">
          <div class="swiper-wrapper" v-if="categorysArr.length">
            <div class="swiper-slide" v-for="(categorys,index) in categorysArr">
              <a href="javascript:" class="link_to_food" v-for="(category,index) in categorys">
                <div class="food_container">
                  <img :src="BaseImageUrl + category.image_url">
                </div>
                <span>{{category.title}}</span>
              </a>
            </div>
          </div>
          <div v-else>
            <img src="./images/home_back.svg" alt="">
          </div>
          <!-- 分页组件-->
          <div class="swiper-pagination"></div>
        </div>
      </nav>

      <!--首页附近商家-->
      <v-ShopList></v-ShopList>
    </section>
</template>

<script>
  import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
  import ShopList from '../../components/ShopList/ShopList.vue'

  import Swiper from 'swiper'
  import 'swiper/dist/css/swiper.min.css'
  import {mapState} from 'vuex'

  export default {

    data() {
      return {
        BaseImageUrl: 'https://fuss10.elemecdn.com'
      }
    },

    components:{
      'v-HeaderTop':HeaderTop,
      'v-ShopList':ShopList
    },

    mounted(){

      this.$store.dispatch('getCategorys')
      this.$store.dispatch('getAddress')

    },

    computed:{
      ...mapState(['address', 'categorys', 'userInfo']),

      /*
      根据categorys一维数组生成一个2维数组
      小数组中的元素个数最大是8
       */
      categorysArr () {
        const {categorys} = this
        // 准备空的2维数组
        const arr = []
        // 准备一个小数组(最大长度为8)
        let minArr = []
        // 遍历categorys
        categorys.forEach(c => {
          // 如果当前小数组已经满了, 创建一个新的
          if (minArr.length === 8) {
            minArr = []
          }
          // 如果minArr是空的, 将小数组保存到大数组中
          if (minArr.length === 0) {
            arr.push(minArr)
          }
          // 将当前分类保存到小数组中
          minArr.push(c)
        })
        return arr
      }
    },

    watch:{     //起到监视的作用
      categorys(value){
        // 监视数据更新之后，再进行界面的展示，界面展示之后再进行分页组件的显示

        // 界面更新就立即创建Swiper对象
        this.$nextTick(() => {// 一旦完成界面更新, 立即调用(此条语句要写在数据更新之后)
          // 创建一个Swiper实例对象, 来实现轮播
          new Swiper('.swiper-container', {
            loop: true, // 可以循环轮播
            // 如果需要分页器
            pagination: {
              el: '.swiper-pagination',
            },
          })
        })
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

  @import "../../assets/stylus/mixins.styl";

  .home  //首页
    width 100%
    overflow hidden
    .home_nav
      bottom-border-1px(#e4e4e4)
      margin-top 45px
      height 200px
      background #fff
      .swiper-container
        width 100%
        height 100%
        .swiper-wrapper
          width 100%
          height 100%
          .swiper-slide
            display flex
            justify-content center
            align-items flex-start
            flex-wrap wrap
            .link_to_food
              width 25%
              .food_container
                display block
                width 100%
                text-align center
                padding-bottom 10px
                font-size 0
                img
                  display inline-block
                  width 50px
                  height 50px
              span
                display block
                width 100%
                text-align center
                font-size 13px
                color #666
        .swiper-pagination
          >span.swiper-pagination-bullet-active
            background #02a774

</style>
