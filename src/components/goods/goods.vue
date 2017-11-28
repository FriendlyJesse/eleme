<template>
  <div class="goods">
    <div class="menu-warpper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <div>
            <span class="icon" v-if="item.type > 0" :class="classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-warpper">

    </div>
  </div>
</template>

<script>
  export default {
    data()
    {
      return {
        goods: {},
        classMap:
        [
          'decrease',
          'discount',
          'special',
          'invoice',
          'guarantee'
        ],
      }
    },
    created()
    {
      this.$http.get('/api').then(res => this.goods = res.data.goods);
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .goods
  {
    display: flex;
    width: 100%;
    position: absolute;
    top: 174px;
    bottom: 46px;
    overflow: hidden;

    .menu-warpper
    {
      flex: 0 0 80px;
      width: 80px;
      background: #f3f5f7;

      .menu-item
      {
        display: flex;
        align-items: center;
        height: 54px;
        width: 56px;
        line-height: 14px;
        margin: 0 12px;
        font-size: 0;
        @include border-1px(rgba(7, 17, 27, .1));

        .icon
        {
          display: inline-block;
          width: 12px;
          height: 12px;
          vertical-align: middle;
          margin-right: 2px;

          &.decrease
          {
            @include bg-img('decrease_3');
          }
          &.discount
          {
            @include bg-img('discount_3');
          }
          &.guarantee
          {
            @include bg-img('guarantee_3');
          }
          &.invoice
          {
            @include bg-img('invoice_3');
          }
          &.special
          {
            @include bg-img('special_3');
          }
        }
        .text
        {
          width: 56px;
          vertical-align: middle;
          font-size: 12px;
        }
      }
    }
    .foods-warpper
    {
      flex: 1;
    }
  }
</style>
