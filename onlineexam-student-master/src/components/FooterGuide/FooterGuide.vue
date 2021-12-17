<template>
<!--  <head>-->
<!--    <link rel="stylesheet" href="//at.alicdn.com/t/font_3024431_clhx7psk90e.css">-->
<!--  </head>-->
  <footer class="footer_guide border-1px">
    <link rel="stylesheet" href="//at.alicdn.com/t/font_3024431_clhx7psk90e.css">
    <div class="guide_item" @click="goto('/home')" :class="{on: isCurrent('/home')}">
      <span class="item_icon">
        <i class="iconfont icon-shouye"></i>
      </span>
      <span>首页</span>
    </div>

    <div class="guide_item" @click="goto('/wrong')" :class="{on: isCurrent('/wrong')}">
      <span class="item_icon">
        <i class="iconfont icon-cuotiku"></i>
      </span>
      <span>错题</span>
    </div>
    <div class="guide_item" @click="goto('/discussion')" :class="{on: isCurrent('/discussion')}">
      <span class="item_icon">
        <i class="iconfont icon-taolunqu"></i>
      </span>
      <span>讨论</span>
    </div>
    <div class="guide_item" @click="goto('/search')" :class="{on: isCurrent('/search')}">
      <span class="item_icon">
        <i class="iconfont iconsousuo"></i>
      </span>
      <span>搜索</span>
    </div>

    <div class="guide_item" @click="goto('/profile')" :class="{on: isCurrent('/profile')}">
      <span class="item_icon">
        <i class="iconfont icon-wo"></i>
      </span>
      <span>我的</span>
    </div>
  </footer>
</template>

<script>
export default {
  name: '',
  data () {
    return {}
  },
  methods: {
    goto (path) {
      this.$router.replace(path)
    },
    isCurrent (path) {
      // console.log(this.$route.path)
      return this.$route.path === path
    }
  }
/*    watch: {
      '$route' (to, from) {
        console.log(to.path.length);
        console.log(from.path.length);
        if(to.path.length > from.path.length) {
          this.$router.isBack = false
        } else {
          this.$router.isBack = true
        }
      }
    } */
}
</script>

<style lang="stylus" type="text/stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixins.styl"
  .footer_guide
    //顶部有白色的边框
    top-border-1px(#e4e4e4)
    position fixed
    z-index 100
    left 0
    right 0
    bottom 0
    background-color #fff
    width 100%
    height 50px
    display flex
    box-shadow 0px 0px 1px rgba(0,0,0,.5)
    .guide_item
      display flex
      flex 1
      text-align center
      flex-direction column
      align-items center
      margin 5px
      color #999999
      &.on
        color #02a774
      span
        font-size 12px
        margin-top 2px
        margin-bottom 2px
        .iconfont
          font-size 22px
</style>
