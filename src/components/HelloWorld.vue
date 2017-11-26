<template>
  <div class="setting">
    <header class="mui-bar mui-bar-nav">
      <a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
      <h1 class="mui-title">设置</h1>
    </header>
    <section class="setting__content mui-content">
      <ul class="mui-table-view">
        <li class="mui-table-view-cell">
          <a class="mui-navigate-right">语言设置language</a>
        </li>
        <li class="mui-table-view-cell">
          <a class="mui-navigate-right">语言设置language</a>
        </li>
        <li class="mui-table-view-cell">
          <a class="mui-navigate-right">语言设置language</a>
        </li>
        <li class="mui-table-view-cell">
          <a class="mui-navigate-right">语言设置language</a>
        </li>
        <li class="mui-table-view-cell">
          <a class="mui-navigate-right">语言设置language</a>
        </li>
      </ul>
      <transition :name="transitionName">
        <router-link to="/A" tag="button" class="radius child-view">{{btn}}</router-link>
      </transition>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      btn: '退出账号',
      transitionName: 'slide-left'
    }
  },
  watch: {
    '$route' (to, from) {
      const toDepth = to.path.split('/').length
      const fromDepth = from.path.split('/').length
      this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped rel="stylesheet/scss">
  .setting
  {
    font-size: 1.4rem;

    &__content
    {
      a
      {
        font-size: 1.4rem;
      }
      .radius
      {
        width: calc(100% - 30px);
        background: red;
        color: #fff;
        margin-top: 10px;
      }
    }

    .fade-enter-active, .fade-leave-active {
      transition: opacity .5s ease;
    }
    .fade-enter, .fade-leave-active {
      opacity: 0
    }
    .child-view {
      position: absolute;
      transition: all .5s cubic-bezier(.55,0,.1,1);
    }
    .slide-left-enter, .slide-right-leave-active {
      opacity: 0;
      -webkit-transform: translate(30px, 0);
      transform: translate(30px, 0);
    }
    .slide-left-leave-active, .slide-right-enter {
      opacity: 0;
      -webkit-transform: translate(-30px, 0);
      transform: translate(-30px, 0);
    }
  }
</style>
