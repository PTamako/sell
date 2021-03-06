<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul>
          <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}"
              @click="selectMenu(index,$event)">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
          </li>
        </ul>
      </div>
      <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li v-for="item in goods" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li @click="selectFood(food,$event)" v-for="food in item.foods" class="food-item border-1px">
                <div class="icon">
                  <img :src="food.icon" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span>月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">¥{{food.price}}</span>
                    <span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartcontrol :food="food" @cart-add="cartAdd"></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price='seller.deliveryPrice'
                :min-price="seller.minPrice"></shopcart>
    </div>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import shopcart from './../../components/shopcart/shopcart'
  import cartcontrol from './../../components/cartcontrol/cartcontrol'
  import food from './../../components/food/food'

  const ERR_OK = 0;
  export default {
    name: "goods",
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods')
        .then((res) => {
          res = res.body;
          if (res.errno === ERR_OK) {
            this.goods = res.data;
            this.$nextTick(() => {
              this._initScroll();
              this._calculateHeight();
            });
          }
        })
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    methods: {
      selectFood(food, event) {
        if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.food.show();
      },
      // 体验优化,异步执行下落动画
      cartAdd(el) {
        this.$nextTick(() => {
          this.$refs['shopcart'].drop(el);
        });
      },
      _initScroll() {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodScroll.on("scroll", (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodsWrapper
          .getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$refs.foodsWrapper
          .getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodScroll.scrollToElement(el, 300);
      }
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import './../../common/stylus/mixin.styl'
  .goods
    display flex
    overflow hidden
    position absolute
    top 174px
    bottom 46px
    width 100%
    .menu-wrapper
      flex 0 0 80px
      width 80px
      .menu-item
        display table
        height 54px
        width 56px
        line-height 14px
        &.current
          color: #035e9b
          font-weight 700
          position relative
          font-size 14px
          z-index 10
          margin-top -1px
          .text
            border-none()
        .text
          text-align center
          display table-cell
          width: 56px
          vertical-align middle
          border-1px(rgba(7, 17, 27, .1))
          font-size: 12px
          .icon
            display inline-block
            vertical-align top
            height 12px
            width 12px
            margin-right 2px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.guarantee
              bg-image('guarantee_3')
            &.invoice
              bg-image('invoice_3')
            &.special
              bg-image('special_3')
    .foods-wrapper
      flex 1
      .title
        padding-left 14px
        height: 26px
        line-height 26px
        border-left 2px solid #d9dde1
        font-size 12px
        color: rgb(147, 153, 159)
      .food-item
        display flex
        margin 18px
        padding-bottom 18px
        border-1px(rgba(7, 17, 27, .1))
        &:last-child
          border-none()
          margin-bottom 0
        .icon
          flex: 0 0 57px
          margin-right 10px
        .content
          flex 1
          .name
            font-size 14px
            height 14px
            line-height: 14px
            color rgb(7, 17, 27)
            margin 2px 0 8px 0
          .desc, .extra
            margin-bottom 8px
            line-height: 12px
            font-size 12px
            color rgb(147, 153, 159)
          .desc
            margin-bottom 8px
            line-height 12px
          .extra
            &.count
              margin-right 12px
          .price
            font-weight 700
            line-height 24px
            .now
              margin-right 8px
              font-style: 14px
              color: rgb(240, 20, 20)
            .old
              text-decoration line-through
              font-size 10px
              color: rgb(147, 153, 159)
          .cartcontrol-wrapper
            position absolute
            right 0
            bottom: 12px
</style>
