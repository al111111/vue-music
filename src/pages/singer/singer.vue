<template>
  <singer-list :data="singerList" class="singer-list"></singer-list>
</template>

<script>
  import {getSingerList} from 'api/singer'
  import {ERR_OK} from 'api/config'
  import Singer from 'common/js/singer'
  import SingerList from 'components/list/list'
  export default {
    name: 'singer',
    data () {
      return {
        singerList: []
      }
    },
    components: {
      SingerList
    },
    created () {
      this._getSingerList()
    },
    methods: {
      _getSingerList () {
        getSingerList().then((res) => {
          if (res.code === ERR_OK) {
            this.disposeSingerList(res.data.list)
          }
        })
      },
      disposeSingerList (list) {
        const HOT_NAME = '热门'
        const HOT_SINGER_LEN = 10
        const map = {
          hot: {
            title: HOT_NAME,
            items: []
          }
        }
        list.forEach((item, index) => {
          if (index < HOT_SINGER_LEN) {
            map.hot.items.push(new Singer({
              id: item.Fsinger_mid,
              name: item.Fsinger_name
            }))
          }
          const key = item.Findex
          if (!map[key]) {
            map[key] = {
              title: key,
              items: []
            }
          }
          map[key].items.push(new Singer({
            id: item.Fsinger_mid,
            name: item.Fsinger_name
          }))
        })
        let hot = []
        let ret = []
        for (let key in map) {
          let val = map[key]
          if (val.title.match(/[a-zA-z]/)) {
            ret.push(val)
          } else if (val.title === HOT_NAME) {
            hot.push(val)
          }
        }
        ret.sort((a, b) => {
          return a.title.charCodeAt(0) - b.title.charCodeAt(0)
        })
        this.singerList = hot.concat(ret)
      }
    }
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
@import '../../assets/stylus/mixin.styl'
  .singer-list
    overflow: hidden
    screenFull(88px)
</style>