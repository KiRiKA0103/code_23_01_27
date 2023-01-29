<template>
  <div class="app-container">
    <!-- Header头部区域 -->
    <Header></Header>
    <Goods v-for="item in list" :key="item.id" :id="item.id" :title="item.goods_name" :imgUrl="item.goods_img"
      :state="item.goods_state" :price="item.goods_price" :count="item.goods_count" @state_change="getNewState">
      <Counter :p_count="item.goods_count" :p_id="item.id" @count_change="getNewCount(item, $event)"></Counter>
    </Goods>

    <Footer :amt="fullPrice" :isFull="fullState" @full_state_change="getNewFullState">
    </Footer>
  </div>
</template>

<script>


import axios from 'axios'

import Header from '@/components/Header/Header.vue'

import Goods from '@/components/Goods/Goods.vue'

import Footer from '@/components/Footer/Footer.vue'

import Counter from '@/components/Counter/Counter.vue'

import bus from '@/components/eventBus.js'

export default {
  data() {
    return {
      // 存储商品列表数据
      list: []
    }
  },
  components: {
    Header,
    Goods,
    Footer,
    Counter
  },
  methods: {
    // 封装请求列表数据方法
    async initCartList() {
      const { data: res } = await axios.get('https://www.escook.cn/api/cart')
      console.log(res);
      if (res.status === 200) {
        this.list = res.list
      }
    },
    getNewState(e) {
      console.log(e);
      this.list.some(item => {
        if (item.id === e.id) {
          item.goods_state = e.value
          return true
        }

      })
    },
    getNewFullState(e) {
      console.log(e);
      this.list.forEach(item => {
        item.goods_state = e
      });
    },
    getNewCount(item, e) {
      item.goods_count = e
    }
  },
  computed: {
    fullState() {
      return this.list.every(item => item.goods_state)
    },
    // 已勾选商品总价
    fullPrice() {
      return this.list.filter(item => item.goods_state).reduce((total, item) => {
        return total += item.goods_price * item.goods_count
      }, 0)
    },
  },
  created() {
    this.initCartList();
    bus.$on('countChange', e => {
      this.list.some(item => {
        if (item.id === e.id) {
          item.goods_count = e.value
          return true
        }
      })
    })
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
