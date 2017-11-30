<template>
  <div class="goods">
    <div class="menu-warpper" ref="menuWrapper">
      <ul>
        <li v-for="(item, index) in goods" class="menu-item" :class="{current: currentIndex == index}" @click="selectMenu(index, $event)">
          <div>
            <span class="icon" v-if="item.type > 0" :class="classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-warpper" ref="foodsWrapper">
      <ul>
        <li class="foods-list" v-for="item in goods" ref="foodList">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item" v-for="food in item.foods">
              <div class="icon">
                <img :src="food.icon" />
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">{{'月售' + food.sellCount + '份'}}</span><span>{{'好评率' + food.rating + '%'}}</span>
                </div>
                <div class="price">
                  <span class="now">{{'￥' + food.price}}</span><span class="old" v-if="food.oldPrice">{{'￥' + food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shop-cart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopCart from '../shopCart/shopCart';

  export default {
    props:
    {
      seller: Object
    },
    components:
    {
      shopCart
    },
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
        listHeight: [],
        scrollY: 0
      }
    },
    created()
    {
//      let _this = this;
//      mui.ajax('http://study.onebound.cn/jesse/mock/',{
//        dataType:'json',//服务器返回json格式数据
//        type:'get',//HTTP请求类型
//        headers:{'Content-Type':'application/json'},
//        success:function(data)
//        {
//          _this.goods = data.goods;
//          _this.$nextTick(() => {
//            _this._initScroll();
//            _this._calculateHeight();
//          });
//        },
//        error:function(xhr,type,errorThrown){
//          //异常处理；
//          console.log(type);
//        }
//      });
      this.$http.get('/api').then((res) =>
      {
          this.goods = res.data.goods;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
      });
    },
    computed:
    {
      currentIndex()
      {
        for (let i = 0; i < this.listHeight.length; i++)
        {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      }
    },
    methods:
    {
      selectMenu(index, event)
      {
        if (!event._constructed)
        {
          return;
        }
        let foodList = this.$refs.foodList;
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _initScroll()
      {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        });
        this.foodsScroll.on('scroll', (pos) =>
        {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight()
      {
        let foodList = this.$refs.foodList;
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++)
        {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
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
        padding: 0 12px;
        font-size: 0;
        @include border-1px(rgba(7, 17, 27, .1));

        &.current
        {
          position: relative;
          margin-top: -1px;
          background: #fff;
          font-weight: 700;

          .text
          {
            border-bottom: none;
          }
        }
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

      .title
      {
        padding-left: 14px;
        height: 26px;
        line-height: 26px;
        border-left: 2px solid #d9dde1;
        font-size: 12px;
        color: rgb(147, 153, 159);
        background: #f3f5f7;
      }
      .food-item
      {
        display: flex;
        margin: 18px;
        padding-bottom: 18px;

        @include border-1px(rgba(7, 17, 27, .1));
        &:last-child
        {
          margin-bottom: 0;

          &::after
          {
            display: none;
          }
        }
        .icon
        {
          flex: 0 0 57px;
          margin-right: 10px;

          img
          {
            width: 57px;
            height: 57px;
          }
        }
        .content
        {
          flex: 1;

          .name
          {
            margin: 2px 0 8px;
            height: 14px;
            line-height: 14px;
            font-size: 14px;
          }
          .desc, .extra
          {
            line-height: 10px;
            font-size: 10px;
            color: rgb(147, 157, 159);
          }
          .desc
          {
            margin-bottom: 8px;
            line-height: 12px;
          }
          .extra
          {
            .count
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
        }
      }
    }
  }
</style>
