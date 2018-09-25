<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" @click="decreaseCart" v-show="food.count>0">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'

  export default {
    name: "cartcontrol",
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {}
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-decrease
      display: inline-block
      padding: 6px
      &.move-transition
        opacity 1
        transform translate3d(0, 0, 0)
        .inner
          display inline-block
          line-height 24px
          font-size 24px
          color: rgb(0, 160, 220)
    .cart-count
      display: inline-block
      font-size 12px
      line-height: 24px
      text-align center
      width 12px
      vertical-align top
      padding-top 6px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      font-size 24px
      padding: 6px
      line-height 24px
      color: rgb(0, 160, 220)
</style>
