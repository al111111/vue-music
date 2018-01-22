<template>
  <div class="list-continar">
    <h2 class="list-title">热门歌单推荐</h2>
    <scroll :data="discList" class="list-song">  
      <div>
        <dl v-for="item in discList" :key="item.dissid" class="disc-list">
          <dd class="list-img">
            <img v-lazy="item.imgurl" :alt="item.creator.name" width="60" height="60">
          </dd>
          <dt class="song-desc">
            <h3 class="title" v-html="item.creator.name"></h3>
            <span class="desc" v-html="item.dissname"></span>
          </dt>
        </dl>
      </div>
      <div class="loading-container" v-show="!discList.length">
        <loading :title="'歌单正在加载中~'"></loading>
      </div>
    </scroll>
  </div>
</template>

<script>
  import Scroll from 'components/scroll/scroll'
  import Loading from 'components/loading/loading'
  export default {
    name: 'hotList',
    props: {
      discList: Array
    },
    components: {
      Scroll,
      Loading
    },
    data () {
      return {}
    }
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
@import '../../assets/stylus/mixin.styl'
  .list-continar
    display: flex
    flex-direction: column
    position: relative
  .list-title
    line-height: 65px
    width: 100%
    text-align: center
    font-size: $font-size-medium
    color: $color-theme
  .list-song
    flex: 1
    overflow: hidden
    .disc-list
      display: flex
      jusitify-content: center
      align-items: center
  .list-img
    padding: 15px
  .song-desc
    height: 85px
    margin:0 10px
    padding-right: 10px
    .title
      no-wrap()
      line-height: 30px
      font-size: $font-size-medium
      color: $color-text-ll
    .desc
      line-height: 20px
      font-size: $font-size-medium
      color: $color-text-d
  .loading-container
    position: absolute
    top: 50%
    width: 100%
    transform: translateY(-50%)
</style>