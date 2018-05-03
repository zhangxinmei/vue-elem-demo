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
          <span class="icon" :class="classMsp[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrap">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
    </div>
    <div class="herder-bg">
      <img :src="seller.avatar" alt="">
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    seller: {
      type: Object
    }
  },
  created() {
    this.classMsp = ["decrease", "discount", "special", "invoice", "guarantee"];
  }
};
</script>

<style lang="less" rel='stylesheet/less'>
.header {
  position: relative;
  color: #fff;
   background-color: rgba(7, 17, 27, 0.2);
  .content-wrap {
    position: relative;
    padding: 24px 12px 18px 24px;
    .avater {
      display: inline-block;
      width: 64px;
      img {
        width: 100%;
        border-radius: 2px;
      }
    }
    .content {
      display: inline-block;
      margin-left: 16px;
      .title {
        margin: 2px 0 8px 0;
        .brand {
          display: inline-block;
          vertical-align: top;
          width: 30px;
          height: 18px;
          background-image: url(./img/brand@2x.png);
          background-size: 30px 18px;
          background-repeat: no-repeat;
        }
        .name {
          font-size: 16px;
          line-height: 18px;
          margin-left: 6px;
        }
      }
      .desc {
        margin-bottom: 10px;
        line-height: 12px;
        font-size: 12px;
      }
      .support {
        .icon {
          display: inline-block;
          vertical-align: top;
          margin-right: 4px;
          width: 12px;
          height: 12px;
          background-size: 12px 12px;
          background-repeat: no-repeat;
          &.decrease {
            background-image: url(./img/decrease_1@2x.png);
          }
          &.discount {
            background-image: url(./img/discount_1@2x.png);
          }
          &.guarantee {
            background-image: url(./img/guarantee_1@2x.png);
          }
          &.invoice {
            background-image: url(./img/invoice_1@2x.png);
          }
          &.special {
            background-image: url(./img/special_2@3x.png);
          }
        }
        .text {
          vertical-align: top;
          line-height: 12px;
          font-size: 10px;
        }
      }
    }
    .support-count {
      position: absolute;
      right: 12px;
      bottom: 18px;
      padding: 0 8px;
      line-height: 24px;
      border-radius: 14px;
      background-color: rgba(0, 0, 0, 0.2);
      text-align: center;
      .count {
        font-size: 10px;
      }
      .icon-keyboard_arrow_right {
        font-size: 10px;
      }
    }
  }
  .bulletin-wrap {
    height: 28px;
    line-height: 28px;
    padding: 0 22px 0 12px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: rgba(7, 17, 27, 0.2);
    &::after {
      content: "";
      height: 100%;
      display: inline-block;
      vertical-align: middle;
    }
    .bulletin-title {
      display: inline-block;
      vertical-align: middle;
      width: 22px;
      height: 12px;
      background-image: url(./img/bulletin@2x.png);
      background-size: 22px 12px;
      background-repeat: no-repeat;
    }
    .bulletin-text {
      display: inline-block;
      vertical-align: middle;
      padding: 0 4px;
      font-size: 10px;
    }
  }
  .herder-bg{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
    img{
      width: 100%;
      height: 100%;
    
    }
  }
}
</style>
