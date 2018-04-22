<template>
  <div class="header">
    <div class="content-wrap">
      <div class="avater">
        <img :src="seller.avatar" alt="">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="desc">{{seller.description}}/{{seller.deliveryTime}}分钟送答</div>
        <div class="support" v-if="seller.supports">
          <span class="icon"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
    </div>
    <div class="bulletin-wrap"></div>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props:{
    seller:{
      type:Object
    }
  }
}
</script>

<style lang="less" rel='stylesheet/less'>
.header{
  color: #fff;
  background: #000;
  .content-wrap{
    padding: 24px 12px 18px 24px;
    .avater{
      display: inline-block;
      width: 64px;
      img{
        width: 100%;
      }
    }
    .content{
      display: inline-block;
    }
  }
}
</style>
