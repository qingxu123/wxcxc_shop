<template>
	<view class="cart-caontainer" v-if="cart.length !==0">
    <!-- 收货地址组件 -->
    <my-address></my-address>
    <!-- 商品列表的标题区域 -->
		<view class="cart-title">
		  <uni-icons type="shop" size="18"></uni-icons>
      <text class="cart-title-text">购物车</text>
		</view>
    
    <!-- 循环渲染购物车中的商品信息 -->

    <uni-swipe-action>
       <block v-for="(goods,i) in cart" :key="i">
        <uni-swipe-action-item :options="options" @click="swipeItemClickHandler(goods)">
         <my-goods @num-change="numberChangeHandler" :goods="goods" :show-num="true" :show-radio="true" @radio-change="radioChangeHandler"></my-goods>
      </uni-swipe-action-item>
       </block>
    </uni-swipe-action>
    
    <!-- 使用自定义的结算组件 -->
    <my-settle></my-settle>
	</view>
  
  <!-- 空白购物车区域 -->
  <view class="empty-cart" v-else>
    <text class="tip-text">空空如也~</text>
  </view>
</template>

<script>
  import badgeMix from '@/mixins/tabbar-badge.js'
  import {mapState, mapMutations} from 'vuex'
	export default {
    mixins:[badgeMix],
    computed:{
      ...mapState('m_cart', ['cart'])
    },
		data() {
			return {
				options:[{
          text:'删除',
          style:{
            backgroundColor:'#C00000'
          }
        }]
			};
		},
   methods:{
     ...mapMutations('m_cart',['updateGoodsState','updateGoodsCount','removeGoodsById']),
     radioChangeHandler(e){
       // console.log(e)
       this.updateGoodsState(e)
     },
     numberChangeHandler(e){
       this.updateGoodsCount(e)
     },
     swipeItemClickHandler(goods){
      this.removeGoodsById(goods.goods_id)
     }
   }
	}
</script>

<style lang="scss">
  .cart-caontainer{
    padding-bottom: 50px;
  }
.cart-title{
  height: 40px;
  display: flex;
  align-items: center;
  padding-left: 5px;
  border-bottom: 1px solid #EFEFEF;
  .cart-title-text{
    font-size: 14px;
    margin-left: 10px;
  }
 }
  
  .empty-cart{
    padding-top: 200px;
    text-align: center;
      .tip-text{
        font-size: 12px;
        color: gray;
        
      }
  }
  

</style>
