<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="selectMenu(index,$event)">
          <span class="goods-text">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="tittle">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img :src="food.icon">
              </div>
             <div class="content">
               <h2 class="name">{{food.name}}</h2>
               <p class="desc" style="">{{food.description}}</p>
               <div class="extra">
                 <span>月售{{food.sellCount}}份</span>
                 <span>好评率{{food.rating}}%</span>
               </div>
               <div class="price">
                 <span style="font-size: 12px;color: red">￥{{food.price}}</span>
                 <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
               </div>
               <div class="cartcontrol-wrapper">
                 <cartcontrol :food="food"></cartcontrol>
               </div>
             </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods=" selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice" ref="shopcart" ></shopcart>
  </div>
</template>
<script type="text/ecmascript-6">
  import shopcart from '../shopcart/shopcart'
  import BScroll from 'better-scroll'
  import cartcontrol from '../cartcontrol/cartcontrol'
  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return{
          goods: [],
          listHeight: [],
          scrollY: 0
      }
    },
    components: {
      shopcart,
      cartcontrol
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];//listHeight[length]返回undefined,所以可以用!height2做判断不是最后一个
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0
      },
      selectFoods () {
        let foods = []
        this.goods.forEach((good)=>{
          good.foods.forEach((food)=>{
            if(food.count){
                foods.push(food)
            }
          })
        })
       return foods
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$http.get('/api/goods').then((response)=>{
        response = response.data
        if (response.errno === ERR_OK) {
          this.goods = response.goods
          this.$nextTick(()=>{
            this._initScroll()
            this._calculateHeight()
          })
          }
      })
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper,{
            click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
          click: true,
          probeType: 3
        })
        this.foodsScroll.on('scroll',(pos)=>{
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      _calculateHeight() {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          //获取每个li的高度，放入一个数组中
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu (index,event) {
        if (!event._constructed) {
          return
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index]
        this.foodsScroll.scrollToElement(el, 300)
        console.log(index)
      }
    }
  }
</script>
<style>
.goods{
  display: flex;
  position: absolute;
  top:222px;
  width:100%;
  bottom:40px;
  overflow: hidden;
}
  .menu-wrapper{
    flex: 0 0 80px;
    width:80px;
    background: #f3f5f7;
  }
  .menu-item{
    display: table;
    height:56px;
    width:56px;
    line-height:14px;
    padding:0 12px;
    border-bottom: 1px solid rgba(7,17,27,0.1);
  }
  .menu-item:last-child{
    border: 0;
  }
.icon{
  display: inline-block;
  width:12px;
  height:12px;
  vertical-align: top;
  margin-right:2px;
  background-size: 12px 12px;
  background-repeat: no-repeat;
}
.icon .decrease{
  background-image: url("../header/decrease_1@2x.png");
}
.discount{
  background-image: url("../header/discount_1@2x.png");
}
.special{
  background-image: url("../header/special_1@2x.png");
}
.guarantee{
  background-image: url("../header/guarantee_1@2x.png");
}
.invoice{
  background-image: url("../header/invoice_1@2x.png");
}
  .goods-text{
    display: table-cell;
    width:56px;
    vertical-align: middle;
    font-size: 12px;
  }
  .foods-wrapper{
    flex:1;
  }
  .tittle {
    padding-left: 14px;
    height: 26px;
    line-height: 26px;
    border-left: 2px solid #d9dde1;
    font-size: 12px;
    color: rgb(147, 153, 159);
    background: #f3f5f7;
  }
    .food-item{
      display: flex;
      margin: 18px;
      border-bottom:1px solid rgba(7,17,27,0.1);
      padding-bottom: 18px;
    }
    .foods-item:last-child{
      border: 0;
    }
    .icon{
      flex: 0 0 57px;
      margin-right:10px;
    }
    .content{
      flex: 1;
      position: relative;
    }
  .content .name{
    margin:2px 0 8px 0;
    height:14px;
    line-height: 14px;
    font-size: 14px;
    color:rgba(7,17,27);
    text-align: left;
  }
  .desc,.extra{
    line-height:10px;
    font-size: 10px;
    color: rgb(147,153,159)
  }
  .content .desc{
    margin: 12px 0 0 0;
  }
  .desc{
    margin-bottom:8px;
    line-height:12px;
  }
  .extra{
    line-height: 10px;
  }
  .food-item .icon img{
    width:60px;
    height:60px;
  }
  .extra .count{
    margin-right:12px;
  }
  .extra .price{
    font-weight: 700;
    line-height: 24px;
  }
  .current{
    position: relative;
    z-index:10;
    margin-top: -1px;
    background: #fff;
    font-weight: 700;
  }
  .current .text{
    border: 0;
  }
  .now{
    margin-right:8px;
    font-size:14px;
    color:rgb(240,20,20)
  }
  .old{
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147,153,159);
  }
  .cartcontrol-wrapper{
    position: absolute;
    right:0;
    bottom: 12px;
  }
</style>
