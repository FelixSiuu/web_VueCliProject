<template>
  <div class="app-container">
    <!-- Header頭部區域 -->
    <Header></Header>
    <!-- 循環渲染每一個商品的信息 -->
    <Goods v-for="item in list" 
            :key="item.id" 
            :title="item.goods_name" 
            :imgSrc="item.goods_img" 
            :price="item.goods_price" 
            :select="item.goods_state"
            :goodsId="item.id"
            :goodsCount="item.goods_count"
            @changeState="getNewState">
    </Goods>
    <Footer :fullState="fullState"
            :totalPrice="totalPrice"
            :totalCount="totalCount"
            @stateAll="getStateAll">
    </Footer>
  </div>
</template>

<script>
// 導入需要的組件
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
// 導入axios
import axios from 'axios'
// 導入EventBus
import Bus from '@/components/EventBus.js'
export default {
  data(){
    return { list:[] }
  },
  components: {
    Header,Goods,Footer
  },
  methods:{
    // 封裝請求列表數據方法
    async initCartList(){
      const { data: res } = await axios.get('https://www.escook.cn/api/cart');
      // 需要轉存數據
      // console.log(res);
      if(res.status === 200) this.list = res.list;
      else { alert('get list failed') };
    },
    // 從子組件中獲取的數據：包含復選框的狀態與id e為接收過來的參數值
    getNewState(e){
      // console.log(e);
      this.list.some( item => {
        // 遍歷父組件中的list 若當前項的id與子組件傳遞過來的參數一致 則修改state的值
        if(item.id == e.id){
          item.goods_state = e.checked;
          // 終止後續的循環
          return true;
        }
      });
    },
    getStateAll(e){
      // console.log(e);
      // 可以利用forEach方法 只要全選框狀態發生改變 就遍歷list
      this.list.forEach( item =>  item.goods_state = e )
    },   
  },
  computed:{
    // 動態計算出全選的狀態是true還是false
    // 利用every方法判斷 若全結果為true則返回true 否則返回false
    fullState(){
      return this.list.every( item => item.goods_state );
    },
    totalPrice(){
      // 先filter過濾 再用reduce方法相加
      return this.list.filter(item => item.goods_state).reduce(( total,item) =>  total += item.goods_count * item.goods_price, 0 ); 
       ;
    },
    totalCount(){
      return this.list.filter(item => item.goods_state).reduce((total,item) => total += item.goods_count, 0);
    }
  },
  created(){
    this.initCartList(),
    // 獲取從counter組件得出來的每項商品的數量 然後回傳數據給list
    Bus.$on('newCount',e =>{
      this.list.some(item => {
        if(item.id === e.id){
          item.goods_count = e.itemCount;
          return true;
        };
      });
    });
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
