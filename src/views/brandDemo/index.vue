<template>
  <div class="brand-demo">
    <h3>品牌选择demo</h3>
    <div @click.stop='open'>
      <p>汽车品牌选择</p>
      <p>当前选择的品牌：{{currentBrand.name}}
        <img :src='currentBrand.img'/>
      </p>
    </div>
    <index-selector :selData='selData' :sideData='sideData' v-model='show' :selTitle="title" @currentData='currentData'>
      <div class='nannanna' :slot="item.datach" v-for="item in selData" :key='item.datach'>
        <div v-for="data in item.data" :key='data.name'>{{data.name}}</div>
      </div>
    </index-selector>
  </div>
</template>
<script>
import IndexSelector from '../../components/index-selector'
import carbrand from './carbrand'
export default {
  data () {
    return {
      show: false,
      currentBrand: {},
      selData: carbrand,
      title: '品牌',
      sideData: ['热', '*', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z']
    }
  },
  mounted () {
    console.log(carbrand)
  },
  components: {
    IndexSelector
  },
  methods: {
    open () {
      this.show = true
    },
    currentData (data) {
      this.currentBrand = data
    }
  }
}
</script>
<style lang="scss">
.brand-demo{

}
</style>
