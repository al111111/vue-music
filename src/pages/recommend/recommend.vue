<template>
  <div class="main">
    <slider :slider="slider" class="slider-container"></slider>
    <hot-list :discList="discList" class="hot-list"></hot-list>
  </div>
</template>

<script>
  import {getRecommend, getDiscList} from 'api/recommend'
  import {ERR_OK} from 'api/config'
  import Slider from 'components/slider/slider'
  import HotList from './hotlist'

  export default {
    name: 'recommend',
    components: {
      Slider,
      HotList
    },
    data () {
      return {
        slider: [],
        discList: []
      }
    },
    methods: {
      _getRecommend () {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.slider = res.data.slider
          }
        })
      },
      _getDiscList () {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            this.discList = res.data.list
          }
        })
      }
    },
    created () {
      this._getRecommend()
      this._getDiscList()
    }
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
@import '../../assets/stylus/mixin.styl'
  .main
    display: flex
    flex-direction: column
    screenFull(88px)
    .hot-list
      flex: 1
  .slider-container
    img-padding(40%)
</style>