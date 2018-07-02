<template>
  <div class="ratings">
    <div class="ratings-wrap">
      <div class="overview">
        <div class="overview-lf">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-rt"></div>
      </div>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    seller: {
      type: Object
    }
  }
}
</script>

<style lang="less" rel='stylesheet/less'>
.ratings {
  position: absolute;
  top: 174px;
  bottom: 0;
  width: 100%;
  overflow: hidden;
  .overview {
    display: flex;
    padding: 18px 0;
    .overview-lf {
      padding: 6px 0;
      flex: 0 0 137px;
      width: 137px;
      border-right: 1px solid rgba(7, 17, 27, 0.1);
      text-align: center;
      .score{
        margin-bottom: 6px;
        line-height: 28px;
        font-size: 24px;
        color: rgb(255, 153, 0);
      }
      .title{
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 12px;
        color: rgb(7, 17, 27);
      }
      .rank{
         line-height: 10px;
         font-size: 10px;
         color: rgb(147, 153, 159);
      }
    }
    .overview-rt {
      flex: 1;
    }
  }
}
</style>
