<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <nav class="tab">
      <router-link to="/goods" tag="div" class="tab-item">商品</router-link>
      <router-link to="ratings" tag="div" class="tab-item">评论</router-link>
      <router-link to="seller" tag="div" class="tab-item">商家</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>
  import vHeader from './components/header/header.vue'
export default {
  name: 'app',
  data()
  {
      return {
        seller:{},

      }
  },
  components:
  {
      vHeader
  },
  created()
  {
//    mui.ajax('http://study.onebound.cn/jesse/mock/',{
//      dataType:'json',//服务器返回json格式数据
//      type:'get',//HTTP请求类型
//      headers:{'Content-Type':'application/json'},
//      success:function(data){
//        _this.goods = data.goods[0].name;
//      },
//      error:function(xhr,type,errorThrown){
//        //异常处理；
//        console.log(type);
//      }
//    });
    this.$http.get('/api').then(res => this.seller = res.data.seller);
  },
  mounted()
  {

  }
}
</script>

<style lang="scss" rel="stylesheet/scss">
  @import "common/scss/mixin";

  #app
  {
    .tab
    {
      display: flex;
      height: 40px;
      line-height: 40px;
      @include border-1px(rgba(7, 17, 27, .1));

      &-item
      {
        flex: 1;
        text-align: center;
        font-size: 14px;
        color: rgb(77, 85, 93);

        &.router-link-active
        {
          color: rgb(240, 20, 20);
        }
      }
    }

  }
</style>
