<template>
  <header class="header">
    <div class="c-header">
      <img src="../../common/img/default.jpg" alt="luffy">
      <h1>Luffy's Music</h1>
    </div>
  </header>
</template>

<script>
  export default {
    name: 'c-header'
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
  .header
    display: flex
    height: 44px
    justify-content: center
    align-items: center
    .c-header
      display: flex
      height: 44px
      align-items: center
      img
        height: 38px
        margin: 3px 6px 0 0
      h1
        line-height: 44px
        font-size: $font-size-large
        color: $color-theme
</style>