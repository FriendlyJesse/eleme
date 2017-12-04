<template>
  <div class="cartControl">
    <transition name="move">
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count > 0" @click="food.count--"></div>
    </transition>
    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="add"></div>
  </div>
</template>

<script>
  export default {
    props:
    {
      food: Object
    },
    methods:
    {
      add(event)
      {
        if (!this.food.count)
        {
          this.$set(this.food, 'count', 1);
        }
        else
        {
          this.food.count++;
        }
        this.$emit('add', event.target);
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  .cartControl
  {
    font-size: 0;

    .cart-decrease, .cart-add
    {
      display: inline-block;
      padding: 6px;
      font-size: 24px;
      line-height: 24px;
      color: rgb(0, 160, 220);
    }
    .cart-count
    {
      display: inline-block;
      vertical-align: top;
      width: 12px;
      padding-top: 6px;
      line-height: 24px;
      text-align: center;
      font-size: 10px;
      color: rgb(147, 153, 159);
    }
    .move-enter-active, .move-leave-active
    {
      transition: .4s linear;
      opacity: 1;
      transform: translate3D(0, 0, 0) rotate(0);
    }
    .move-enter, .move-leave-to
    {
      opacity: 0;
      transform: translate3D(24px, 0, 0) rotate(180deg);
    }
  }
</style>
