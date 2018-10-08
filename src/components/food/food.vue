<template>
  <transition name="move">
    <div class="food" v-show="showFlag">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div>
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  export default {
    name: "food",
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      show() {
        this.showFlag = true;
      }
    },
    data() {
      return {
        showFlag: false
      }
    },
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food
    position: fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #fff
    transform translate3d(0, 0, 0)
    &.move-enter-activity, &.move-leave-activity
      transition all 0.2s linear
    &.move-enter, &.move-leave-activity
      transform translate3d(100%, 0, 0)
    .image-header
      position: relative
      width: 100%
      height: 0
      padding-top 100%
      img
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
      .background
</style>
