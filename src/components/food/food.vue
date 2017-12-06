<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image" />
          <div class="back" @click="showFlag = false">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">{{'月售' + food.sellCount + '份'}}</span>
            <span class="rating">{{'好评率' + food.rating}}</span>
          </div>
          <div class="price">
            <span class="now">{{'￥' + food.price}}</span><span class="old" v-if="food.oldPrice">{{'￥' + food.oldPrice}}</span>
          </div>
          <div class="cartControl-warpper">
            <cart-control @add="addFood" :food="food"></cart-control>
          </div>
          <transition name="fade">
            <div class="buy" v-show="!food.count || food.count == 0" @click="addFirst">加入购物车</div>
          </transition>
        </div>
        <split></split>
        <div class="info">
          <div class="title">商品信息</div>
          <div class="text">{{food.info}}</div>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <rating-select :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings" @select="selectRating" @toggle="toggleContent"></rating-select>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li class="rating-item" v-for="rating in food.ratings" v-show="needShow(rating.rateType, rating.text)">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img width="12" height="12" class="avatar" :src="rating.avatar" />
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <p class="text">
                  <span :class="{'icon-thumb_up': rating.rateType == 0, 'icon-thumb_down': rating.rateType == 1}"></span>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-reting" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
  import BScroll from 'better-scroll';
  import cartControl from '../cartControl/cartControl';
  import split from '../split/split';
  import ratingSelect from '../ratingSelect/ratingSelect';
  import {formatDate} from '../../common/js/date';

  const POSITIVE = 0,
        NEGATIVE = 1,
        ALL = 2;

  export default {
    props:
    {
      food: Object
    },
    components:
    {
      cartControl,
      split,
      ratingSelect
    },
    data()
    {
      return {
        showFlag: false,
        scroll: null,
        selectType: ALL,
        onlyContent: true,
        desc:
        {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      }
    },
    filters:
    {
      formatDate(time)
      {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    methods:
    {
      show()
      {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = false;
        this.$nextTick(() =>
        {
          if (!this.scroll)
          {
            this.scroll = new BScroll(this.$refs.food,
            {
              click: true
            });
          }
        })
      },
      addFirst(event)
      {
        this.$emit('add', event.target);
        this.$set(this.food, 'count', 1);
      },
      addFood()
      {
        this.$emit('add', event.target);
      },
      selectRating(type)
      {
        this.selectType = type;
        this.$nextTick(() =>
        {
          this.scroll.refresh();
        });
      },
      toggleContent()
      {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() =>
        {
          this.scroll.refresh();
        });
      },
      needShow(type, text)
      {
        if (this.onlyContent && !text)
        {
          return false;
        }
        if (this.selectType === ALL)
        {
          return true;
        }
        else
        {
          return type === this.selectType;
        }
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .food
  {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    width: 100%;
    z-index: 30;
    background: #fff;

    .img-header
    {
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%;

      img
      {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
      .back
      {
        position: absolute;
        top: 10px;
        left: 0;

        .icon-arrow_lift
        {
          display: block;
          padding: 10px;
          font-size: 20px;
          color: #fff;
        }
      }
    }
    .content
    {
      padding: 18px;
      position: relative;

      .title
      {
        line-height: 14px;
        margin-bottom: 8px;
        font-size: 14px;
        font-weight: 700;
        color: rgb(7, 17, 27);
      }
      .detail
      {
        margin-bottom: 18px;
        height: 10px;
        line-height: 10px;
        font-size: 0;

        .sell-count, .rating
        {
          font-size: 10px;
          color: rgb(147, 153, 159);
        }
        .sell-count
        {
          margin-right: 12px;
        }
      }
      .price
      {
        font-weight: 700;
        line-height: 24px;

        .now
        {
          margin-right: 8px;
          font-size: 14px;
          color: rgb(240, 20, 20);
        }
        .old
        {
          text-decoration: line-through;
          font-size: 10px;
          color: rgb(147, 157, 159);
        }
      }
      .cartControl-warpper
      {
        position: absolute;
        right: 12px;
        bottom: 12px;
      }
      .buy
      {
        position: absolute;
        right: 18px;
        bottom: 18px;
        z-index: 10;
        height: 24px;
        line-height: 24px;
        padding: 0 12px;
        box-sizing: border-box;
        font-size: 10px;
        border-radius: 12px;
        color: #fff;
        background: rgb(0, 160, 220);
      }
    }
    .info
    {
      padding: 18px;

      .title
      {
        line-height: 14px;
        margin-bottom: 6px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
      .text
      {
        line-height: 24px;
        padding: 0 8px;
        font-size: 12px;
        color: rgb(77, 85, 93);
      }
    }
    .rating
    {
      padding-top: 18px;

      .title
      {
        line-height: 14px;
        margin-left: 18px;
        font-size: 14px;
        color: rgb(7, 17, 27);
      }
      .rating-wrapper
      {
        padding: 0 18px;

        .rating-item
        {
          position: relative;
          padding: 16px 0;
          @include border-1px(rgba(7, 17, 27, .1));

          .user
          {
            position: absolute;
            right: 0;
            top: 16px;
            line-height: 12px;
            font-size: 0;

            .name
            {
              display: inline-block;
              vertical-align: top;
              font-size: 10px;
              color: rgb(147, 153, 159);
              margin-right: 6px;
            }
            .avatar
            {
              border-radius: 50%;
            }
          }
          .time
          {
            line-height: 12px;
            font-size: 10px;
            color: rgb(147, 153, 159);
            margin-bottom: 6px;
          }
          .text
          {
            line-height: 16px;
            font-size: 12px;
            color: rgb(7, 17, 27);

            .icon-thumb_up, .icon-thumb_down
            {
              line-height: 16px;
              margin-right: 4px;
              font-size: 12px;
            }
            .icon-thumb_up
            {
              color: rgb(0, 160, 220);
            }
            .icon-thumb_down
            {
              color: rgb(147, 153, 159);
            }
          }
        }
        .no-reting
        {
          padding: 16px 0;
          font-size: 12px;
          color: rgb(147, 153, 159);
        }
      }
    }
  }

  .fade-enter-active, .fade-leave-active
  {
    opacity: 1;
    transition: all 0.2s;
  }
  .fade-enter, .fade-leave-active
  {
    opacity: 0;
    z-index: -1;
  }
  .move-enter-active, .move-leave-active {
    transition: all 0.2s linear;
  }
  .move-enter, .move-leave-active /* .fade-leave-active in below version 2.1.8 */ {
    transform: translate3d(100%, 0, 0);
  }
</style>
