<template>
  <div class="star" :class="'star-'+size">
    <span class="star-item" v-for="item in StarClasses" :class="item"></span>
  </div>
</template>

<script>
    // 类名常量
    const CLASS_ON = 'on'
    const CLASS_HALF = 'half'
    const CLASS_OFF = 'off'

    export default {
        data() {
            return {}
        },

        props:{
            score: Number,
            size: Number
        },

        computed:{
          /*
          *   3.5   3  1  1
          *   3.2   3  0  2
          * */
          StarClasses(){
            const {score} = this
            const starClassList = []
            const starClassInteger = Math.floor(score)
            for(let i = 0; i < starClassInteger; i++){
              starClassList.push(CLASS_ON)
            }

            if (score*10 - starClassInteger*10 >= 5){
              starClassList.push(CLASS_HALF)
            }

            while(starClassList.length < 5){
              starClassList.push(CLASS_OFF)
            }

            return starClassList
          }
        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .star //2x图 3x图
    float left
    font-size 0
    margin-right 15px
    .star-item
      display inline-block
      background-repeat no-repeat
    &.star-48
      .star-item
        width 20px
        height 20px
        margin-right 22px
        background-size 20px 20px
        &:last-child
          margin-right: 0
        &.on
          bg-image('./images/stars/star48_on')
        &.half
          bg-image('./images/stars/star48_half')
        &.off
          bg-image('./images/stars/star48_off')
    &.star-36
      .star-item
        width 15px
        height 15px
        margin-right 6px
        background-size 15px 15px
        &:last-child
          margin-right 0
        &.on
          background-image url('./images/stars/star36_on@2x.png')
        &.half
          background-image url('./images/stars/star36_half@2x.png')
        &.off
          background-image url('./images/stars/star36_off@2x.png')
    &.star-24
      .star-item
        width 10px
        height 10px
        margin-right 3px
        background-size 10px 10px
        &:last-child
          margin-right 0
        &.on
          background-image url('./images/stars/star24_on@2x.png')
          opacity 1
        &.half
          background-image url('./images/stars/star24_half@2x.png')
        &.off
          background-image url('./images/stars/star24_off@2x.png')
</style>
