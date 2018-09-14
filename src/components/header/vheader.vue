<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bullentin-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: "header",
    props: {
      seller: {
        type: Object,
        default: null
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl"
  .header
    color: #fff
    background #7e8c8d
    .content-wrapper
      padding 24px 12px 18px 24px
      font-size 0
      .avatar
        display inline-block
        font-size 14px
        vertical-align top
        img
          border-radius 3px
      .content
        display inline-block
        margin-left 16px
        font-size 14px
        .title
          margin 2px 0 8px 0
          .brand
            display inline-block
            vertical-align top
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .name
            margin-left 6px
            font-size 16px
            line-height 18px
            font-weight bold
        .description
          margin-bottom 10px
          font-size 14px
          line-height 14px
        .support
          font-size 12px
          line-height 12px
          .icon
            height 12px
            width 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            display inline-block
            &.decrease
              bg-image('decrease_1')
</style>
