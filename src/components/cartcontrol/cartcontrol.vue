<template>
  <div class="cartcontrol">
    <transition name="move">
    <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
      <span class="inner icon iconfont icon-jian"></span>
    </div>
      </transition>
    <div class="cart-count icon " v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon iconfont icon-jia" @click="addCart"></div>
  </div>
</template>
<script type="text/ecmascript-6">
import Vue from 'vue'
export default {
  props:{
    food: {
      type: Object
    }
  },
  created () {
    console.log(this.food)
  },
  methods: {
    addCart (event) {
      if(!event._constructed){
        return
      }
      console.log("click")
      if(!this.food.count){
        Vue.set(this.food,'count',1)
        this.food.count = 1
      } else{
       this.food.count++
      }
    },
    decreaseCart(event){
      if(!event._constructed){
        return
      }
      if(this.food.count){
        this.food.count--
      }
    }
  }
}
</script>
<style>
.cartcontrol {
  font-size: 0;
}
.cartcontrol>.cart-decrease,.cart-count,.cart-add{
  display: inline-block;
}
.cart-add{
  line-height: 24px;
  font-size: 24px;
  padding:6px;
  color:rgb(0,160,220)
}
.cart-count {
  display: inline-block;
  vertical-align: top;
  width: 12px;
  padding-top: 6px;
  line-height: 24px;
  text-align: center;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.inner {
  display: inline-block;
  line-height: 24px;
  font-size: 20px;
  color: rgb(0, 160, 220);
  transition: all 0.4s linear;
  transform: rotate(0deg);
}
.cart-decrease{
  display: inline-block;
  padding:6px;
  transition: all 0.4s linear;
}
/* 开始过渡阶段,动画出去阶段 */
.move-enter-active,.move-leave-active{
  transition: all 0.5s ease-out;
}
/* 进入开始 */
.move-enter{
  transform: translateX(80px) rotate(180deg);

  opacity: 0;
}
/* 出去终点 */
.move-leave-active{
  transform: translateX(30px) rotate(0deg);
  opacity: 1;
}
</style>
