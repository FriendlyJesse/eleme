<template>
  <div class="star" :class="'star-' + this.size">
    <span class="star-item" v-for="itemClass in itemClasses" :class="itemClass"></span>
  </div>
</template>

<script>

  const CLS_ON = 'on',
        CLS_HALF = 'half',
        CLS_OFF = 'off',
        LENGTH = 5;

  export default {
    props:
    {
      size: Number,
      score: Number
    },
    computed:
    {
      itemClasses()
      {
        let result = [],
            score = Math.floor(this.score * 2) / 2,  //取拥有.5的数或证书
            hasDecimal = score % 1 !== 0,  //求是否有小数
            integer = Math.floor(score);  //求整数

        for (let i = 0; i < integer; i++)
        {
          result.push(CLS_ON);
        }
        if (hasDecimal)
        {
          result.push(CLS_HALF);
        }
        while (result.length < LENGTH)
        {
          result.push(CLS_OFF);
        }
        return result;
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .star
  {
    font-size: 0;

    .star-item
    {
      display: inline-block;
    }
    &.star-48
    {
      .star-item
      {
        width: 20px;
        height: 20px;
        margin-right: 22px;

        &:last-child
        {
          margin-right: 0;
        }
        &.on
        {
          @include bg-img(star48_on);
        }
        &.half
        {
          @include bg-img(star48_half);
        }
        &.off
        {
          @include bg-img(star48_off);
        }
      }
    }
    &.star-36
    {
      .star-item
      {
        width: 15px;
        height: 15px;
        margin-right: 6px;

        &:last-child
        {
          margin-right: 0;
        }
        &.on
        {
          @include bg-img(star36_on);
        }
        &.half
        {
          @include bg-img(star36_half);
        }
        &.off
        {
          @include bg-img(star36_off);
        }
      }
    }
    &.star-24
    {
      .star-item
      {
        width: 10px;
        height: 10px;
        margin-right: 3px;

        &:last-child
        {
          margin-right: 0;
        }
        &.on
        {
          @include bg-img(star24_on);
        }
        &.half
        {
          @include bg-img(star24_half);
        }
        &.off
        {
          @include bg-img(star24_off);
        }
      }
    }
  }
</style>
