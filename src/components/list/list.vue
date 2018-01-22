<template>
  <scroll :data="data" 
          class="list-view" 
          ref="listView" 
          :listenScroll="true" 
          :probeType="3"
          @scroll="scroll">
    <ul>
      <li v-for="group in data" class="list-group" ref="listGroup">
        <h2 class="list-group-title">{{group.title}}</h2>
        <ul class="list-group-container">
          <li v-for="item in group.items" class="list-group-item">          
            <img v-lazy="item.avatar" :alt="item.name" class="avatar">
            <span class="name">{{item.name}}</span>
          </li>
        </ul>
      </li>
    </ul>

    <ul class="shortcut-list" @touchstart="handleShortCutTouchStart" @touchmove.stop.prevent="handleShortCutTouchMove">
      <li v-for="(item, index) in shortCutList" 
          :key="index" 
          :data-name="index" 
          :class="{'current': currentIndex === index}">
        {{item}}
      </li>
    </ul>
    <div class="list-fixed" v-show="fixedTitle" ref="fixed">
      <h3 class="fixed-title">{{fixedTitle}}</h3>
    </div>
    <loading v-show="!data.length" class="loading"></loading>
  </scroll>
</template>

<script>
  import Scroll from 'components/scroll/scroll'
  import Loading from 'components/loading/loading'
  import {getData} from 'common/js/dom'
  const CUT_HEIGHT = 18
  const TITLE_HEIGHT = 30

  export default {
    created () {
      this.touch = {}
      this.listHeight = []
    },
    props: {
      data: {
        type: Array,
        default: []
      }
    },
    data () {
      return {
        scrollY: -1,
        currentIndex: 0,
        diff: -1
      }
    },
    components: {
      Scroll,
      Loading
    },
    computed: {
      shortCutList () {
        return this.data.map((group) => {
          return group.title.substr(0, 1)
        })
      },
      fixedTitle () {
        if (this.scrollY > 0) {
          return ''
        }
        return this.data[this.currentIndex] ? this.data[this.currentIndex].title : ''
      }
    },
    watch: {
      data () {
        this.$nextTick(() => {
          this._calculateHeight()
        })
      },
      scrollY (newY) {
        const listHeight = this.listHeight
        if (-newY <= 0) {
          this.currentIndex = 0
          return
        }
        for (let i = 0; i < listHeight.length - 1; i++) {
          let height1 = listHeight[i]
          let height2 = listHeight[i + 1]
          if ((-newY >= height1 && -newY < height2)) {
            this.currentIndex = i
            this.diff = height2 + newY
            return
          }
        }
        this.currentIndex = listHeight.length - 2
      },
      diff (newVal) {
        if (newVal < TITLE_HEIGHT && newVal > 0) {
          let h = TITLE_HEIGHT - newVal
          this.$refs.fixed.style.top = (88 - h) + 'px'
        } else {
          this.$refs.fixed.style.top = 88 + 'px'
        }
      }
    },
    methods: {
      handleShortCutTouchStart (e) {
        const cutIndex = getData(e.target, 'name')
        let firstTouch = e.targetTouches[0].pageY
        this.touch.y1 = firstTouch
        this.touch.cutIndex = cutIndex
        this._scrollTo(cutIndex)
      },
      handleShortCutTouchMove (e) {
        let firstTouch = e.targetTouches[0].pageY
        this.touch.y2 = firstTouch
        let delta = (this.touch.y2 - this.touch.y1) / CUT_HEIGHT | 0
        let cutIndex = delta + parseInt(this.touch.cutIndex)
        cutIndex = cutIndex > this.$refs.listGroup.length ? this.$refs.listGroup.length : cutIndex
        this._scrollTo(cutIndex)
      },
      scroll (pos) {
        this.scrollY = pos.y
      },
      _scrollTo (index) {
        if (!index && index !== 0) {
          return
        }
        index = index < 0 ? 0 : (index > this.listHeight.length - 2 ? this.listHeight.length - 2 : index)
        this.scrollY = -this.listHeight[index]
        this.$refs.listView.scrollToElement(this.$refs.listGroup[index], 0)
      },
      _calculateHeight () {
        const list = this.$refs.listGroup
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < list.length; i++) {
          let item = list[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      }
    }
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
@import '../../assets/stylus/mixin.styl'
  .list-group-title
    line-height: 30px
    padding-left: 20px 
    font-size: $font-size-small
    color: $color-text-l
    background: $color-highlight-background
  .list-group-container
    padding-bottom: 20px
  .list-group-item
    display: flex
    padding: 20px 0 0 30px
    height: 50px
    align-items: center
    .avatar
      width: 50px
      height: 50px
      border-radius: 100%
    .name
      margin-left: 20px
      font-size: $font-size-medium
      color: $color-text-l
  .shortcut-list
    position: absolute
    top: 50%
    right: 0
    transform: transLateY(-50%)
    z-index: 20
    width: 20px
    padding: 20px 0
    border-radius: 10px
    text-align: center
    background: $color-background-d
    li
      padding: 3px
      line-height: 1
      font-size: $font-size-small
      color: $color-text-l
    .current
      color: $color-theme
  .list-fixed
    position: fixed
    top: 88px
    width: 100%
    .fixed-title
      line-height: 30px
      padding-left: 20px 
      font-size: $font-size-small
      color: $color-text-l
      background: $color-highlight-background
  .loading
    position: absolute
    top: 50%
    left: 50%
    transform: transLate3d(-50%, -50%, 0)
</style>