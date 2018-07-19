<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>元
            </div>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="bullentin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrap border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
      </div>
      <ul class="supports" v-if="seller.supports">
        <li class="supports-item border-1px" v-for="(item,index) in seller.supports" :key="index">
          <span class="icon" :class="classMsp[seller.supports[index].type]"></span>
          <span class="text">{{seller.supports[index].description}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
import star from '../star/star'
import split from '../split/split'
import BScroll from 'better-scroll'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star,
    split
  },
  data() {
    return {}
  },
  created() {
    this.classMsp = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  mounted() {
    this.$nextTick(()=>{
      this._initScroll();
    })
  },
  methods:{
    _initScroll() {
      if(!this.scroll){
        this.scroll=new BScroll(this.$refs.seller,{
          click:true
        })
      }else{
        this.scroll.refresh();
      }
    }
  }
}
</script>

<style lang="less" rel='stylesheet/less'>
@import '../../common/less/mixin.less';
.seller {
  position: absolute;
  top: 174px;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  .title {
    margin-bottom: 8px;
    line-height: 14px;
    color: rgb(7, 17, 27);
    font-size: 14px;
  }
  .overview {
    padding: 18px;
    .desc {
      padding-bottom: 18px;
      line-height: 14px;
      color: rgb(7, 17, 27);
      .border-1px;
      .star {
        display: inline-block;
        margin-right: 8px;
        vertical-align: top;
      }
      .text {
        display: inline-block;
        margin-right: 12px;
        line-height: 18px;
        vertical-align: top;
        font-size: 10px;
        color: rgb(77, 85, 93);
      }
    }
    .remark {
      display: flex;
      .block {
        flex: 1;
        padding-top: 20px;
        text-align: center;
        border-right: 1px solid rgba(7, 17, 27, 0.1);
        &:last-child {
          border-right: none;
        }
        h2 {
          margin-bottom: 4px;
          line-height: 10px;
          font-size: 10px;
          color: rgb(7, 17, 27);
        }
        .content {
          line-height: 24px;
          font-size: 10px;
          color: rgb(7, 17, 27);
          .stress {
            font-size: 24px;
          }
        }
      }
    }
  }
  .bullentin {
    padding: 18px 18px 0 18px;
    .content-wrap {
      padding: 0 12px 16px 12px;
      .border-1px;
      .content {
        line-height: 24px;
        font-size: 12px;
        color: rgb(240, 20, 20);
      }
    }
  }
  .supports {
    .supports-item {
      padding: 18px 12px;
      .border-1px;
      .icon {
        display: inline-block;
        vertical-align: top;
        margin-right: 4px;
        width: 12px;
        height: 12px;
        background-size: 12px 12px;
        background-repeat: no-repeat;
        &.decrease {
          background-image: url(./img/decrease_4@2x.png);
        }
        &.discount {
          background-image: url(./img/discount_4@2x.png);
        }
        &.guarantee {
          background-image: url(./img/guarantee_4@2x.png);
        }
        &.invoice {
          background-image: url(./img/invoice_4@2x.png);
        }
        &.special {
          background-image: url(./img/special_4@2x.png);
        }
      }
      .text{
        line-height: 16px;
        font-size: 12px;
        color: rgb(7, 17, 27);
      }
    }
  }
}
</style>
