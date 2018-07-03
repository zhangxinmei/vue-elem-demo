<template>
  <div class="ratings">
    <div class="ratings-wrap">
      <div class="overview">
        <div class="overview-lf">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-rt">
          <div class="score-wrap">
            <div class="title">服务态度</div>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrap">
            <div class="title">商品评分</div>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrap">
            <span class="title">送达时间</span>
            <span class="delivery-time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star
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
      .score {
        margin-bottom: 6px;
        line-height: 28px;
        font-size: 24px;
        color: rgb(255, 153, 0);
      }
      .title {
        margin-bottom: 8px;
        line-height: 12px;
        font-size: 12px;
        color: rgb(7, 17, 27);
      }
      .rank {
        line-height: 10px;
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
    }
    .overview-rt {
      flex: 1;
      padding: 6px 0 6px 24px;
      .score-wrap {
        margin-bottom: 8px;
        line-height: 18px;
        font-size: 0;
        &::after {
          display: inline-block;
          vertical-align: middle;
          content: '';
          height: 100%;
        }
        .title {
          display: inline-block;
          vertical-align: middle;
          font-size: 12px;
          color: rgb(7, 17, 27);
        }
        .star {
          display: inline-block;
          vertical-align: middle;
          font-size: 12px;
          margin: 0 10px;
        }
        .star.star-36 .star-item {
          margin-right: 8px;
        }
        .score {
          display: inline-block;
          vertical-align: middle;
          line-height: 18px;
          font-size: 12px;
          color: rgb(255, 153, 0);
        }
      }
      .delivery-wrap {
        .title {
          line-height: 18px;
          font-size: 12px;
          color: rgb(7, 17, 27);
        }
        .delivery-time {
          margin-left: 12px;
          font-size: 12px;
          color: rgb(147, 153, 159);
        }
      }
    }
  }
}
@media only screen and (max-width: 320px) {
  .ratings {
    .overview {   
      .overview-lf {
        padding: 6px 0;
        flex: 0 0 110px;
        width: 110px;     
      }
      .overview-rt {
        padding: 6px 0 6px 6px;
      }
    }
  }
}
</style>
