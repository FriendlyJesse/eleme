<template>
  <div class="shopCart">
    <div class="content">
      <div class="content-left">
        <div class="logo-warpper">
          <div class="logo" :class="{highlight: totalCount > 0}">
            <i class="icon-shopping_cart"></i>
          </div>
          <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{highlight: totalPrice > 0}">{{'￥' + totalPrice}}</div>
        <div class="desc">{{'另需配送费￥' + deliveryPrice + '元'}}</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="{enough: this.minPrice - this.totalPrice <= 0}">{{payDesc}}</div>
      </div>
    </div>
    <div class="ball-container">
      <div v-for="ball in balls">
        <transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
          <div class="ball" v-show="ball.show">
            <div class="inner"></div>
          </div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props:
    {
      //接收数据，使用计算属性计算价格
      selectFoods:
      {
        type: Array,
        default()
        {
            return [{price: 30, count: 1}];
        }
      },
      deliveryPrice:
      {
        type: Number,
        default: 0
      },
      minPrice:
      {
        type: Number,
        default: 0
      }
    },
    data()
    {
      return {
        balls:
        [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: []
      }
    },
    computed:
    {
      totalPrice()
      {
        let total = 0;
        this.selectFoods.forEach(food => total += food.price * food.count)
        return total;
      },
      totalCount()
      {
        let count = 0;
        this.selectFoods.forEach(food => count += food.count)
        return count;
      },
      payDesc()
      {
        if (this.minPrice - this.totalPrice > 0)
        {
          return `还差￥${this.minPrice - this.totalPrice}元起送`;
        }
        else
        {
          return '去结算';
        }
      }
    },
    methods:
    {
      drop(el)
      {
        for (let i; i < this.balls.length; i++)
        {
          let ball = this.balls[i];
          if (!ball.show)
          {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      },
      beforeDrop(el)
      {
        let count = this.balls.length;
        while (count--)
        {
          let ball = this.balls[count];
          if (ball.show)
          {
            let rect = ball.el.getBoundingClientRect();
            let x = rect.left - 32;
            let y = -(window.innerHeight - rect.top);
            el.style.display = '';
            el.style.webkitTransform = `translate3d(0,${y}px,0)`;
            el.style.transform = `translate3d(0,${y}px,0)`;
          }
        }
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  .shopCart
  {
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 50;
    width: 100%;
    height: 48px;

    .content
    {
      display: flex;
      background: #141d27;

      &-left
      {
        flex: 1;
        font-size: 0;

        .logo-warpper
        {
          display: inline-block;
          position: relative;
          top: -10px;
          margin: 0 12px;
          padding: 6px;
          width: 56px;
          height: 56px;
          box-sizing: border-box;
          vertical-align: top;
          border-radius: 50%;
          background: #141d27;

          .logo
          {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: #2b343c;
            text-align: center;

            &.highlight
            {
              background: rgb(0, 160, 220);

              .icon-shopping_cart
              {
                color: #fff;
              }
            }
            .icon-shopping_cart
            {
              line-height: 48px;
              font-size: 24px;
              color: #80858a;
            }
          }
          .num
          {
            position: absolute;
            top: 0;
            right: 0;
            width: 24px;
            height: 16px;
            line-height: 16px;
            text-align: center;
            border-radius: 16px;
            font-size: 9px;
            font-weight: 700;
            color: #fff;
            background: rgb(240, 20, 20);
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
          }
        }
        .price
        {
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          box-sizing: border-box;
          padding-right: 12px;
          border-right: 1px solid rgba(255, 255, 255, .1);
          font-size: 16px;
          font-weight: 700;
          color: rgba(255, 255, 255, .4);

          &.highlight
          {
            color: #fff;
          }
        }
        .desc
        {
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin: 12px 0 0 12px;
          color: rgba(255, 255, 255, .4);
          font-size: 10px;
        }
      }
      &-right
      {
        flex: 0 0 105px;
        width: 105px;

        .pay
        {
          height: 48px;
          line-height: 48px;
          text-align: center;
          font-size: 12px;
          color: rgba(255, 255, 255, .4);
          font-weight: 700;
          background: #2b333b;

          &.enough
          {
            color: #fff;
            background: #00b43c;
          }
        }
      }
    }
    .ball-container
    {
      .ball
      {
        position: fixed;
        left: 32px;
        bottom: 32px;
        z-index: 200;
        transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41);

        .inner
        {
          width: 16px;
          height: 16px;
          border-radius: 50%;
          background: rgb(0, 160, 220);
          transition: all 0.4s linear
        }
      }
    }
  }
</style>
