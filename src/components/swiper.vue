<template>
  <div class="swiper" :style="swiperSize">
    <ul :style="listWrapper">
      <li class="swiper-list" v-for="(item,index) in data" :key="index">
        <a :href="item.href">
          <img :style="swiperSize" :src="item.imgUrl" alt />
        </a>
      </li>
    </ul>
    <ul class="swiper-pagination">
      <li
        v-for="(item,index) in data"
        @click="changeImg(index)"
        :class="{'active': activeIndex === index}"
        :key="index"
      ></li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'swiper',
  props: {
    data: {
      type: Array,
      default () {
        return []
      }
    },
    height: {
      type: Number,
      default: 500
    },
    width: {
      type: Number,
      default: 1240
    },
    delay: {
      type: Number,
      default: 5000
    }
  },
  computed: {
    swiperSize () {
      return {
        width: `${this.width}px`,
        height: `${this.height}px`
      }
    },
    listWrapper () {
      return {
        width: `${this.data.length * this.width}px`,
        height: `${this.height}px`,
        transform: `translateX(-${this.activeIndex * this.width}px)`,
        transitionDuration: '1s'
      }
    }
  },
  data () {
    return {
      activeIndex: 0,
      timer: null
    }
  },
  mounted () {
    this.setTimer()
  },
  methods: {
    setTimer () {
      clearInterval(this.timer)
      this.timer = setInterval(() => {
        if (this.activeIndex === this.data.length - 1) {
          this.activeIndex = 0
        } else {
          this.activeIndex++
        }
      }, this.delay)
    },
    changeImg (index) {
      this.activeIndex = index
    }
  }
}
</script>
<style lang="less" scoped>
.swiper {
  position: relative;
  overflow: hidden;
  margin: 0 auto;

  .swiper-list {
    display: inline-block;
  }

  .swiper-pagination {
    position: absolute;
    left: 30%;
    bottom: 20px;

    li {
      display: inline-block;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      margin: 0 5px;
      background-color: #fff;
      cursor: pointer;

      &.active {
        background-color: transparent;
        border: 1px solid #fff;
      }
    }
  }
}
</style>
