<template>
  <div class="singer-detail"></div>
</template>

<script>
  import {mapGetters} from 'vuex'
  import {getSingerDetail} from 'api/singer'
  import {ERR_OK} from 'api/config'
  import {createSong} from 'common/js/song'
  export default {
    name: 'singer-detail',
    data () {
      return {
        songs: []
      }
    },
    computed: {
      ...mapGetters([
        'singer'
      ])
    },
    methods: {
      _getDetail () {
        const singermid = this.singer.id ? this.singer.id : this.$router.history.current.params.id
        getSingerDetail(singermid).then(this.getSingerDetailSucc.bind(this))
      },
      getSingerDetailSucc (res) {
        if (ERR_OK === res.code) {
          this._normalizeSongs(res.data.list)
        }
      },
      _normalizeSongs (list) {
        let ret = []
        list.forEach((item) => {
          let {musicData} = item
          ret.push(createSong(musicData))
        })
      }
    },
    created () {
      this._getDetail()
    }
  }
</script>

<style scoped lang="stylus">
@import '../../assets/stylus/variable.styl'
  
  .singer-detail
    position: fixed
    z-index: 100
    top: 0
    left: 0
    right: 0
    bottom: 0
    background: $color-background
</style>