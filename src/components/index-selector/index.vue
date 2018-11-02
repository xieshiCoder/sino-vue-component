<template>
  <div class="sino-sel" :class="{'sino-sel-in': value}">
    <section class="sino-sel-nav" ref='selnav'>
      <div class="sino-sel-nav-back"  @click.stop='close'>
        <i class="left-arrow1"></i>
        <i class="left-arrow2"></i>
      </div>
      {{selTitle}}
    </section>
    <section class="sino-sel-main noscroll-bar">
      <div class="sino-sel-item" :data-ch='item.datach' v-for="item in selData" :key='item.datach'>
        <div class="sino-sel-item-title" :class="{'sino-sel-item-title-bj':item.titleBj}">{{item.title}}</div>
        <slot v-if='!item.listType' :name='item.datach'></slot>
        <div class="sino-sel-item-grid" v-if='item.listType==="grid"'>
          <div class="sino-sel-item-grid-col" v-for="ddd in item.data" :key='ddd.name' @click="biringBack(ddd)">
            <img class="" v-if="ddd.img" :src="ddd.img" alt="">
            <span v-if="ddd.name">{{ddd.name}}</span>
          </div>
        </div>
        <div class="sino-sel-item-line" v-if='item.listType==="line"'>
          <div class="sino-sel-item-line-col" v-for="ddd in item.data" :key='ddd.name' @click="biringBack(ddd)">
            <img class="" v-if="ddd.img" :src="ddd.img" alt="">
            <span v-if="ddd.name" :class="{'sino-sel-item-line-text': !ddd.img}">{{ddd.name}}</span>
          </div>
        </div>
      </div>
    </section>
    <div class="sino-sel-aside" :class="{'sino-sel-aside-in': !value}">
      <ul class="sino-sel-aside-list" :style='topAdapter' @touchstart.prevent='start' @touchmove.prevent='move' @touchend.prevent='end'>
        <li v-for="item in sideData" :key='item'>{{item}}</li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      touching: false,
      initData: {},
      currentChar: '',
      history: [],
      sideBarStyle: {}
    }
  },
  props: {
    value: {
      type: Boolean,
      default: false
    },
    selData: {
      type: Array
    },
    sideData: {
      type: Array
    },
    selTitle: {
      type: String
    }
  },
  computed: {
    topAdapter () {
      return {
        marginTop: this.sideBarStyle.paddingTop + 'px'
      }
    }
  },
  mounted () {
    this.sideBarAdapter()
  },
  methods: {
    close () {
      this.$emit('input', false)
    },
    sideBarAdapter () {
      const htmlFontSize = document.querySelector('html').style.fontSize.split('px')[0]
      const winHeight = this.$el.offsetHeight
      const navHeight = document.querySelector('.sino-sel-nav').offsetHeight
      const sideDetailHeight = 35 / 75 * this.sideData.length * htmlFontSize
      const paddingTop = (winHeight - navHeight - sideDetailHeight) / 2
      this.sideBarStyle = { htmlFontSize, winHeight, navHeight, sideDetailHeight, paddingTop }
    },
    /**
     * 指尖触摸开始的回调
     */
    start (e) {
      if (!this.touching) {
        this.touching = !this.touching
        this.$emit('showChar', true)
        this.move(e)
      }
    },
    /**
     * 指尖触摸移动的回调
     */
    move (e) {
      if (this.touching) {
        const boxRect = document.querySelector('.sino-sel-aside-list').getBoundingClientRect()
        const offset = this.calcRelativePosition(e.touches[0].clientY)
        const percent = offset / boxRect.height
        const ch = this.getPositionChar(percent)
        this.updateChar(e.touches[0].clientX, e.touches[0].clientY, ch)
      }
    },
    /**
     * 指尖触摸结束的回调
     */
    end (e) {
      if (this.touching) {
        this.touching = !this.touching
        this.$emit('showChar', false)
      }
    },
    /**
     * 获取当前指尖位置上的字母
     */
    getPositionChar (yPercent) {
      var min = 1
      var max = this.sideData.length
      var index = Math.ceil(yPercent * max)
      if (index < min) {
        index = min
      } else if (index > max) {
        index = max
      }
      return this.sideData[index - 1]
    },
    /**
     * 计算位置
     */
    calcRelativePosition (clientY) {
      const boxRect = document.querySelector('.sino-sel-aside-list').getBoundingClientRect()
      let y = clientY - boxRect.top
      if (y < 0) {
        y = 0
      } else if (y > boxRect.height) {
        y = boxRect.height
      }
      return y
    },
    /**
     * 更新提示字母
     */
    updateChar (clientX, clientY, ch) {
      const target = document.querySelector('.sino-sel-main').querySelector('[data-ch="' + ch + '"]')
      if (target) {
        target.scrollIntoView()
      }
    },
    biringBack (data) {
      this.$emit('currentData', data)
      this.close()
    }
  }
}
</script>
<style lang="scss">
@import './style';
</style>
