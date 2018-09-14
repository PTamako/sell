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
          <span class="icon" :class="classMap[seller.supports[2].type]"></span>
          <span class="text">{{seller.supports[2].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
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
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./../../common/stylus/mixin.styl"
  .header
    color: #fff
    background-color: rgba(0, 0, 0, .5);
    .content-wrapper
      position relative
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
          line-height 12px
          font-size 12px
          .icon
            display inline-block
            vertical-align top
            height 12px
            width 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
      .support-count
        right: 12px
        bottom: 14px
        padding 2px 8px
        height: 24px
        line-height: 24px
        position absolute
        border-radius 14px
        background-color: rgba(0, 0, 0, .2)
        text-align center
        .count
          vertical-align top
          font-size 12px
        .icon-keyboard_arrow_right
          margin-left 2px
          line-height 24px
          font-size 12px

</style>
