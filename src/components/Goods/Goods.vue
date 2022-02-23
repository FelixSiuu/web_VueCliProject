<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input type="checkbox" class="custom-control-input" :id="'cb'+goodsId" :checked="select" @change="stateChange"/>
        <label class="custom-control-label" :for="'cb'+goodsId">
          <!-- 商品的缩略图 -->
          <img :src="imgSrc" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ (price * goodsCount).toFixed(2) }}</span>
        <!-- 商品的数量 -->
        <Counter :goodsCount="goodsCount" :id="goodsId"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter.vue'
export default {
  components:{
    Counter
  },
  props:{
    // 商品的標題
    title:{
      type: String,
      default: ''
    },
    // 商品圖片地址
    imgSrc:{},
    // 商品價格
    price:{
      type: Number,
      default: 0
    },
    // 商品的勾選狀態
    select:{
      type: Boolean,
      default: true
    },
    // id參數為子傳父參數時需用上
    goodsId:{
      required: true,
      type: Number
    },
    goodsCount:{
      type: Number,
      default: 1
    }
  },
  data(){
      return {  }
  },
  methods:{
    // 在子組件中透過點擊復選框 把狀態回傳給父組件以及id 以用作同步更新
    stateChange(e){
      // 透過參數e事件對象可以獲得相關信息
      const newState = { checked: e.target.checked, id: this.goodsId }
      // console.log(newState);
      this.$emit('changeState',newState);
    },
  }
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
