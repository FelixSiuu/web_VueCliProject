<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="des">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ count }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import Bus from '@/components/EventBus.js'
export default {
  props:{
    goodsCount:{
      type: Number,
      default: 1
    },
    id: {
      type: Number,
      required: true
    }
  },
  data(){
    return { count: this.goodsCount }
  },
  methods:{
    // 點擊加號按鈕數量增加 傳參給App.vue
    add(){
      this.count++;
      Bus.$emit('newCount',{ itemCount:this.count, id: this.id});
    },    
    // 點擊減號按鈕數量減少 傳參給App.vue
    des(){
      if(this.count> 1) this.count--;
      Bus.$emit('newCount',{ itemCount:this.count, id: this.id});
    }
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
