<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab bordertop-1px borderbottom-1px">
      <div class="tab-item">
        <a v-link="{path:'/goods'}">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/ratings'}">评论</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/seller'}">商家</a>
      </div>
    </div>
    <router-view :seller="seller" keep-alive></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue';
  import {urlparth} from 'common/js/util.js';

  export default{
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlparth();
            return queryParam.id;
          })()
        }
      };
    },
    created() {
      this.$http.get('/api/data').then((response) => {
        response = response.body;
        response = JSON.parse(response);
        console.log(response);
        if (response !== undefined) {
          this.seller = Object.assign({}, this.seller, response.seller);
        }
      });
    },
    components: {
      'v-header': header
    }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  #app
    .tab
      display: flex
      width: 100%
      height: 40px
      borderbottom-1px(rgba(7, 17, 27, 0.1))
      bordertop-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex: 1
        text-align: center
        & > a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.active
            color: rgb(240, 20, 20)
</style>
