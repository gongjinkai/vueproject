<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
      <router-link to="/goods/" tag="a">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings/" tag="a">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller/" tag="a">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
import header from './components/header/header'
const ERR_OK = 0
  export default {
    data () {
      return {
        seller: {}
      }
    },
    created() {
      this.$http.get('/api/seller').then((response) => {
        response = response.data
        if (response.errno === ERR_OK) {
          this.seller = response.seller
          console.log(this.seller)
        }
      });
    },
    components: {
      'v-header': header
    }
}
</script>

<style>
a{
  display: block;
  text-decoration: none;
  color: rgb(77,85,93);
}
.tab{
  display: flex;
  width:100%;
  height:40px;
  line-height: 40px;
  border-bottom: 1px solid rgba(7,17,27,0.1);
}
.tab-item{
  flex:1;
  text-align: center;
}
.router-link-active {
 color:rgb(240,20,20)
}
</style>
