<template>
  <div class="tab">
    <router-link tag="div" class="tab-item" to="/recommend">
      <span class="tab-link">推荐</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/singer">
      <span class="tab-link">歌手</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/rank">
      <span class="tab-link">排行</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/search">
      <span class="tab-link">搜索</span>
    </router-link>
  </div>
</template>

<script>
  export default {
    name: 'tab'
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
  .tab
    display: flex
    justify-content: space-around
    position: relative
    z-index: 20
    background: $color-background
    .tab-item
      line-height: 44px
      .tab-link
        padding-bottom: 5px
        color: $color-text-l
        font-size: $font-size-medium
      &.router-link-active
        .tab-link
          color: $color-theme
          border-bottom: 2px solid $color-theme
</style>