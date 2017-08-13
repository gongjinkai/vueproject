<template>
  <div class="star" :class="starType">
    <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item" track-by="$index"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGTH = 5;
  const CLS_ON = 'on';
  const CLS_HALF = 'half';
  const CLS_OFF = 'off';

  export default {
    props: {
      size: {
        type: Number
      },
      score: {         // 评价的分数
        type: Number
      }
    },
    computed: {
      starType() {
        return 'star-' + this.size;
      },
      itemClasses() {
        let result = []; // 最终所有星星存放的数组
        let score = Math.floor(this.score * 2) / 2;
        let hasDecimal = score % 1 !== 0; // 小数
        let integer = Math.floor(score); // 整数
        for (let i = 0; i < integer; i++) {
          result.push(CLS_ON);
        }
        if (hasDecimal) {
          result.push(CLS_HALF);
        }
        while (result.length < LENGTH) {
          result.push(CLS_OFF);
        }
      console.log(result);
        return result;
      }
    }
  };
</script>
<style>
  .star{
    font-size: 0;
  }
  .star .star-item{
  display: inline-block;
  background-repeat: no-repeat;
}
  .star-48 .star-item{
    width:20px;
    height:20px;
    margin-right:22px;
    background-size: 20px 20px;
  }
  .star-48 .star-item.on{
    background-image: url("star48_on@2x.png");
  }
  .star-48 .star-item.half{
    background-image: url("star48_half@2x.png");
  }
  .star-48 .star-item.off{
    background-image: url("star48_off@2x.png");
  }
  .star-48-item:last-child{
    margin-right:0;
  }
  .star-36 .star-item{
    width:15px;
    height:15px;
    margin-right:6px;
    background-size: 15px 15px;
  }
  .star-36 .star-item.on{
    background-image: url("star36_on@2x.png");
  }
  .star-36 .star-item.half{
    background-image: url("star36_half@2x.png");
  }
  .star-36 .star-item.off{
    background-image: url("star36_off@2x.png");
  }
  .star-36:last-child{
    margin-right:0;
  }
  .star-24 .star-item{
    width:15px;
    height:15px;
    margin-right:6px;
    background-size: 15px 15px;
  }
  .star-24 .star-item.star-item .on{
    background-image: url("star24_on@2x.png");
  }
  .star-24 .star-item.star-item .half{
    background-image: url("star24_half@2x.png");
  }
  .star-24 .star-item.star-item .off{
    background-image: url("star24_off@2x.png");
  }
  .star-24:last-child{
    margin-right:0;
  }
</style>
