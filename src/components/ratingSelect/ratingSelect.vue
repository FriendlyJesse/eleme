<template>
  <div class="ratingSelect">
    <div class="rating-type">
      <span class="block all" :class="{active: selectType == 2}" @click="select(2)">
        {{desc.all}}<span class="count">{{ratings.length}}</span>
      </span>
      <span class="block positive" :class="{active: selectType == 0}" @click="select(0)">
        {{desc.positive}}<span class="count">{{positive.length}}</span>
      </span>
      <span class="block negative" :class="{active: selectType == 1}" @click="select(1)">
        {{desc.negative}}<span class="count">{{negative.length}}</span>
      </span>
    </div>
    <div class="switch" :class="{on: onlyContent}">
      <span class="icon-check_circle" @click="toggleContent"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>

  const POSITIVE = 0,
        NEGATIVE = 1,
        ALL = 2;

  export default {
    props:
    {
      ratings:
      {
        type: Array,
        default()
        {
          return [];
        }
      },
      selectType:
      {
        type: Number,
        default: ALL
      },
      onlyContent:
      {
        type: Boolean,
        default: false
      },
      desc:
      {
        type: Object,
        default()
        {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    computed:
    {
      positive()
      {
        return this.ratings.filter((rating) =>
        {
          return rating.rateType == POSITIVE;
        })
      },
      negative()
      {
        return this.ratings.filter((rating) =>
        {
          return rating.rateType == NEGATIVE;
        })
      }
    },
    methods:
    {
      select(type)
      {
        this.$emit('select', type);
      },
      toggleContent()
      {
        this.$emit('toggle');
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
  @import "../../common/scss/mixin";

  .ratingSelect
  {
    .rating-type
    {
      padding: 18px 0;
      margin: 0 18px;
      font-size: 0;
      @include border-1px(rgba(7, 17, 27, .1));

      .block
      {
        display: inline-block;
        padding: 8px 12px;
        margin-right: 8px;
        line-height: 16px;
        border-radius: 1px;
        font-size: 12px;
        color: rgb(77, 85, 93);

        &.active
        {
          color: #fff;
        }
        &.all
        {
          background: rgba(0, 160, 220, .2);

          &.active
          {
            background: rgb(0, 160, 220);
          }
        }
        &.positive
        {
          background: rgba(0, 160, 220, .2);

          &.active
          {
            background: rgb(0, 160, 220);
          }
        }
        &.negative
        {
          background: rgba(77, 85, 93, .2);

          &.active
          {
            background: rgb(77, 85, 93);
          }
        }
        .count
        {
          font-size: 8px;
          margin-left: 2px;
        }
      }
    }
    .switch
    {
      padding: 12px 18px;
      line-height: 24px;
      border-bottom: 1px solid rgba(7, 17, 27, .1);
      color: rgb(147, 153, 159);
      font-size: 0;

      &.on
      {
        .icon-check_circle
        {
          color: #00c850;
        }
      }
      .icon-check_circle
      {
        display: inline-block;
        vertical-align: top;
        margin-right: 4px;
        font-size: 24px;
      }
      .text
      {
        font-size: 12px;
      }
    }
  }
</style>
