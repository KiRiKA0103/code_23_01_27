<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ p_count }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import { type } from 'os';
import bus from '@/components/eventBus.js'

export default {
  props: {
    p_count: {
      default: 0,
      type: Number,
    },
    p_id: {
      type: Number,
      required: true,
    }
  },
  data() {
    return {

    }
  },
  methods: {
    sub() {
      if (this.p_count > 1) {
        bus.$emit('countChange', { id: this.p_id, value: this.p_count - 1 })
      }
    },
    add() {
      bus.$emit('countChange', { id: this.p_id, value: this.p_count + 1 })
    },
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
