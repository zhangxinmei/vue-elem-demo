<template>
  <div class="ratings" ref="ratings">
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
      <slpit></slpit>
      <ratingselect @select="selectRating" @toggle="toggleContent" :ratings="ratings" :select-type="selectType" :only-content="onlyContent"></ratingselect>
      <div class="rating-content">
        <ul>
          <li class="rating-item border-1px" v-for="(rating,index) in ratings" :key="index" v-show="needShow(rating.rateType,rating.text)">
            <div class="avator">
              <img :src="rating.avatar" alt="">
            </div>
            <div class="contetn">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrap">
                <star :size="24" :score="rating.score" v-show="rating.deliveryTime">{{rating.deliveryTime}}</star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟</span>
                <p class="text">{{rating.text}}</p>
                <div class="recommend" v-show="rating.recommend&&rating.recommend.length">
                  <i class="icon-thumb_up"></i>
                  <span class="item" v-for="(item,index) in rating.recommend" :key="index">{{item}}</span>
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type='text/ecmascript-6'>
import BScroll from 'better-scroll'
import star from '../star/star'
import slpit from '../split/split.vue'
import ratingselect from '../ratingselect/ratingselect.vue'
import { formatDate } from '../../common/js/date.js'
const ALL = 2
const ERR_OK = 0
export default {
  props: {
    seller: {
      type: Object
    }
  },
  components: {
    star,
    slpit,
    ratingselect
  },
  data() {
    return {
      selectType: ALL,
      onlyContent: true,
      ratings: []
    }
  },
  created() {
    this.axios.get('/api/ratings').then(res => {
      res = res.data
      if (res.errno === ERR_OK) {
        this.ratings = res.data
        //  console.log('ratings',res)
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.ratings, {
            click: true
          })
        })
      }
    })
  },
  filters: {
    formatDate(time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  methods: {
    needShow(type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    },
    toggleContent() {
      this.onlyContent = !this.onlyContent
    },
    selectRating(type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    toggleContent() {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    }
  }
}
</script>

<style lang="less" rel='stylesheet/less'>
@import '../../common/less/mixin.less';
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
  .rating-content {
    padding: 0 18px;
    .rating-item {
      display: flex;
      padding: 18px 0;
      .border-1px;
      .avator {
        flex: 0 0 28px;
        margin-right: 18px;
        width: 28px;
        height: 28px;
        img {
          width: 100%;
          border-radius: 50%;
        }
      }
      .contetn {
        position: relative;
        flex: 1;
        .name {
          margin-bottom: 4px;
          line-height: 12px;
          font-size: 10px;
          color: rgb(7, 17, 27);
        }
        .star-wrap {
          margin-bottom: 6px;
          font-size: 0;
          .star {
            display: inline-block;
            vertical-align: top;
          }
          .delivery {
            display: inline-block;
            margin: 0 0 5px 6px;
            margin-left: 5px;
            font-size: 10px;
            line-height: 12px;
            color: rgb(147, 153, 159);
          }
          .text {
            line-height: 18px;
            font-size: 12px;
            color: rgb(7, 17, 27);
            margin-bottom: 8px;
          }
          .recommend {
            line-height: 16px;
            .icon-thumb_up,
            .item {
              display: inline-block;
              margin: 0 8px 4px 0;
              font-size: 9px;
            }
            .icon-thumb_up {
              color: rgb(0, 160, 220);
            }
            .item {
              padding: 0 6px;
              border: 1px solid rgba(7, 17, 27, 0.1);
              border-radius: 2px;
              color: rgb(147, 153, 159);
              background: #fff;
            }
          }
          .time {
            position: absolute;
            top: 0;
            right: 0;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
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
