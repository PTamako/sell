<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <ul>
        <router-link v-for="(item,index) in Infos" :key="index"
                     :to="{ path: item.path }" tag="li" active-class="active">{{item.name}}
        </router-link>
      </ul>
    </div>
    <div class="content">
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script>
  import VHeader from './components/header/vheader'

  const ERR_OK = 0;
  export default {
    name: 'App',
    components: {
      VHeader
    },
    data() {
      return {
        Infos: [
          {
            name: "商品",
            path: 'goods',
            active: false
          },
          {
            name: "评价",
            path: 'ratings',
            active: false
          },
          {
            name: "商家",
            path: 'seller',
            active: false
          }
        ],
        seller: {}
      };
    },
    methods: {},
    created() {
      this.$http.get('/api/seller')
        .then((res) => {
          res = res.body;
          if (res.errno === ERR_OK) {
            this.seller = res.data;
          }
        })
    },
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      ul
        display: flex
        width: 100%
        height: 40px
        line-height: 40px
        border-1px(rgba(7, 17, 27, .1))
        li
          flex: 1
          text-align: center
          &.active
            color: rgb(240, 20, 20)
</style>
