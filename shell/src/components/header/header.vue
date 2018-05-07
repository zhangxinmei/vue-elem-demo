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
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrap" @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
    </div>
    <div class="herder-bg">
      <img :src="seller.avatar" alt="">
    </div>
    <transition name="fade">
 <div v-show="isDetailShow" class="detial">
      <!--CSS sticky footer -->
      <div class="detail-wrap clearfix">
        <div class="detail-mian">
          <div class="detail-hd">
            <h1>{{seller.name}}</h1>
            <div class="hd-star">
              <star :size="48" :score="seller.score"></star>
            </div>
          </div>
          <headline :titleText="titleTextOffer"></headline>
          <ul class="supports" v-if="seller.supports">
            <li class="supports-item" v-for="(item,index) in seller.supports" :key="index">
              <span class="icon" :class="classMsp[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
          <headline :titleText="titleTextBulletin"></headline>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-cloase" @click="isDetailShow=false">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script type='text/ecmascript-6'>
import star from "../star/star";
import headline from "../title/title";
export default {
  data() {
    return {
      isDetailShow: false,
      titleTextOffer: "优惠活动",
      titleTextBulletin: "商家公告"
    };
  },
  components: {
    star,
    headline
  },
  props: {
    seller: {
      type: Object
    }
  },
  created() {
    this.classMsp = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
  methods: {
    showDetail() {
      this.isDetailShow = true;
    }
  }
};
</script>

<style lang="less" rel='stylesheet/less'>
.header {
  position: relative;
  overflow: hidden;
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
  .herder-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
    img {
      width: 100%;
      height: 100%;
    }
  }
  .detial {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    z-index: 2;
    backdrop-filter: blur(10px);
    background: rgba(7, 17, 27, 0.8);
     &.fade-enter-active, &.fade-leave-active{
        transition: all 0.5s;
     }
     &.fade-enter, &.fade-leave-active{
        opacity: 0;
        background:rgba(7, 17, 27, 0);
     } 
    .detail-wrap {
      width: 100%;
      min-height: 100%;
      .detail-mian {
        margin-top: 64px;
        padding-bottom: 64px;
        .detail-hd {
          line-height: 16px;
          font-size: 16px;
          font-weight: 700;
          text-align: center;
          .hd-star {
            margin-top: 18px;
            padding: 2px 0;
          }
        }
        .supports {
          width: 80%;
          margin: 0 auto;
          .supports-item {
            padding: 0 12px;
            margin-bottom: 12px;
            font-size: 0;
            &:last-child {
              margin-bottom: 0;
            }
            &::after {
              display: inline-block;
              vertical-align: middle;
              content: "";
              height: 100%;
            }
            .icon {
              display: inline-block;
              vertical-align: middle;
              width: 16px;
              height: 16px;
              margin-right: 6px;
              background-repeat: no-repeat;
              background-size: 16px 16px;
              &.decrease {
                background-image: url(./img/decrease_2@2x.png);
              }
              &.discount {
                background-image: url(./img/discount_2@2x.png);
              }
              &.guarantee {
                background-image: url(./img/guarantee_2@2x.png);
              }
              &.invoice {
                background-image: url(./img/invoice_2@2x.png);
              }
              &.special {
                background-image: url(./img/special_2@3x.png);
              }
            }
            .text {
              display: inline-block;
              vertical-align: middle;
              font-size: 12px;
            }
          }
        }
        .bulletin {
          width: 80%;
          margin: 0 auto;
          .content {
            padding: 0 12px;
            line-height: 24px;
            font-size: 12px;
          }
        }
      }
    }
    .detail-cloase {
      position: relative;
      width: 32px;
      height: 32px;
      margin: -64px auto 0 auto;
      clear: both;
      font-size: 32px;
    }
  }
}
</style>
