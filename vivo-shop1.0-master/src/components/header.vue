<template>
  <div class="header">
      <div class="header-left" @click="$router.go(-1)">
          <i class="iconfont icon-zuojiantou"></i>
      </div>
      <div class="header-in">{{title}}</div>
      <div class="header-rigth"></div>
  </div>
</template>

<script>
export default {
  props:[
      "title"
  ]
}
</script>

<style lang="sass" scoped>
@import '../assets/styles/common.scss'

.header
  position: fixed
  width: 100%
  top: 0
  z-index: 1
  height: px2rem(96px)
  line-height: px2rem(96px)
  background: #0d0d0d
  color: #fff

  .header-left
    width: 10%
    height: 100%
    float: left
      
    i 
      font-size: px2rem(36px)
      text-align: center
      display: block

  .header-in
    width: 80%
    float: left
    text-align: center
    font-size: px2rem(28px)

  .header-rigth
    width: 10%
    height: 100%
    float: left

</style>
