<template>
  <div class="page">

    <home v-if="isHomeCur"></home>
    <moments v-else></moments>

    <div class="tab">
      <a :class="{current: isHomeCur}" @click="goNext('home')">首页</a>
      <a :class="{current: !isHomeCur}" @click="goNext('moments')">朋友圈</a>
    </div>

  </div>
</template>

<script>
import home from '@/components/home'
import moments from '@/components/moments'

export default {
  data: {
    tabPage: {
      default: 'home'
    },
    isHomeCur: true
  },

  components: {
    home,
    moments
  },

  methods: {
    goNext(page){
      this.tabPage = page;
      if(page == 'home') this.isHomeCur = true
      else this.isHomeCur = false
    }
  }
}
</script>

<style scoped lang="scss">
.page{
  padding-bottom: 80rpx;
  .tab{
    position: fixed;
    bottom: 0;
    z-index: 10;
    background: #fff;
    display: block;
    width: 100%;
    font-size: 30rpx;
    line-height: 80rpx;
    color: #222;
    border-top: 1px solid #eee;

    a{
      display: inline-block;
      text-align: center;
      width: 50%;
      box-sizing: border-box;

      &:first-child{
        border-right: 1px solid #eee;
      }

      &.current{
        color: pink;
      }
    }
  }
}
</style>
