<template>
  <swiper :options="swiperOption" class="slider-container">
    <swiper-slide v-for="item in slider" :key="item.id">
      <a :href="item.linkUrl">
        <img class="banner-img" :src="item.picUrl" />
      </a>
    </swiper-slide>
    <div class="swiper-pagination"  slot="pagination"></div>
  </swiper>
</template>

<script>
  import 'swiper/dist/css/swiper.css'
  import {swiper, swiperSlide} from 'vue-awesome-swiper'

  export default {
    name: 'recommend',
    props: {
      slider: Array
    },
    components: {
      swiper,
      swiperSlide
    },
    data () {
      return {
        swiperOption: {
          loop: true,
          autoplay: 4000,
          pagination: '.swiper-pagination'
        }
      }
    }
  }
</script>

<style scoped lang="stylus">
  .banner-img
    width: 100%
  .slider-container >>> .swiper-pagination-bullet
  .swiper-pagination-bullet
    background: #fff
  .slider-container >>> .swiper-pagination-bullet-active
  .swiper-pagination-bullet-active
    background: #fff
    width: 20px
    border-radius: 5px
</style>
