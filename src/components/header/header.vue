<template>
  <header class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" />
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description + '/' + seller.deliveryTime + '分钟送达'}}
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="detailShow = true">
        <span class="count">{{seller.supports.length + '个'}}</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="detailShow = true">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" />
    </div>
    <div class="detail" v-show="detailShow">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-warpper">
            <star :size="48" :score="seller.score"></star>
          </div>
        </div>
      </div>
      <div class="detail-close">
        <i class="icon-close" @click="detailShow = false"></i>
      </div>
    </div>
  </header>
</template>

<script>
  import star from '../../components/star/star.vue';

  export default {
    name: 'header',
    props:
    {
      seller: Object
    },
    data ()
    {
      return {
        classMap:
        [
          'decrease',
          'discount',
          'special',
          'invoice',
          'guarantee'
        ],
        detailShow: false
      }
    },
    components:
    {
      star
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .header
  {
    color: #fff;
    background: rgba(7, 17, 27, .5);
    position: relative;
    overflow: hidden;

    .content-wrapper
    {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;

      .avatar
      {
        display: inline-block;
        vertical-align: top;

        img
        {
          width: 64px;
          height: 64px;
          border-radius: 2px;
        }
      }
      .content
      {
        display: inline-block;
        margin-left: 16px;

        .title
        {
          margin: 2px 0 8px 0;

          .brand
          {
            display: inline-block;
            vertical-align: top;
            width: 30px;
            height: 18px;
            @include bg-img(brand);
          }
          .name
          {
            margin-left: 6px;
            font-size: 16px;
            line-height: 18px;
            font-weight: bold;
          }
        }
        .description
        {
          margin-bottom: 10px;
          line-height: 12px;
          font-size: 12px;
        }
        .support
        {
          .icon
          {
            display: inline-block;
            width: 12px;
            height: 12px;
            vertical-align: top;
            margin-right: 4px;

            &.decrease
            {
              @include bg-img('decrease_1');
            }
            &.discount
            {
              @include bg-img('discount_1');
            }
            &.guarantee
            {
              @include bg-img('guarantee_1');
            }
            &.invoice
            {
              @include bg-img('invoice_1');
            }
            &.special
            {
              @include bg-img('special_1');
            }
          }
          .text
          {
            font-size: 12px;
            line-height: 12px;
          }
        }
      }
      .support-count
      {
        position: absolute;
        right: 12px;
        bottom: 14px;
        padding: 0 8px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background: rgba(0, 0, 0, .2);
        text-align: center;

        .count
        {
          vertical-align: top;
          font-size: 10px;
        }
        .icon-keyboard_arrow_right
        {
          font-size: 10px;
          margin-left: 2px;
          line-height: 24px;
        }
      }
    }
    .bulletin-wrapper
    {
      position: relative;
      height: 28px;
      line-height: 28px;
      padding: 0 22px 0 12px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      background: rgba(7, 17, 27, .2);

      .bulletin-title
      {
        display: inline-block;
        width: 22px;
        height: 12px;
        vertical-align: top;
        margin-top: 8px;
        @include bg-img('bulletin');
      }
      .bulletin-text
      {
        vertical-align: top;
        font-size: 10px;
        margin: 0 4px;
      }
      .icon-keyboard_arrow_right
      {
        position: absolute;
        top: 8px;
        right: 12px;
        font-size: 10px;
      }
    }
    .background
    {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: -1;
      filter: blur(10px);

      img
      {
        width: 100%;
        height: 100%;
      }
    }
    .detail
    {
      position: fixed;
      z-index: 100;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      overflow: auto;
      background: rgba(7, 17, 27, .8);

      .detail-wrapper
      {
        width: 100%;
        min-height: 100%;

        .detail-main
        {
          margin-top: 64px;
          padding-bottom: 64px;

          .name
          {
            line-height: 16px;
            font-size: 16px;
            font-weight: 700;
            text-align: center;
          }
          .star-warpper
          {
            margin-top: 18px;
            padding: 2px 0;
            text-align: center;
          }
        }
      }
      .detail-close
      {
        position: relative;
        width: 32px;
        height: 32px;
        margin: -64px auto 0;
        clear: both;
        font-size: 32px;
      }
    }
  }
</style>
