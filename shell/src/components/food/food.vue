<template>
  <transition name="move">
    <div class="food" v-show="showFoodDetil" ref="food">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image" alt="">
          <div class="back" @click="back">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrap" v-if="food.count>0">
            <cartcontrol :food="food"></cartcontrol>
          </div>
          <transition name="fade">
            <div class="buy" v-if="!food.count||food.count===0" @click="addFirstFood">加入购物车</div>
          </transition>
        </div>
        <slpit v-show="food.info"></slpit>
        <div class="info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <div class="text">{{food.info}}</div>
        </div>
        <slpit v-show="food.info"></slpit>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect @select="selectRating" @toggle="toggleContent" :ratings="food.ratings" :select-type="selectType" :only-content="onlyContent" :desc="desc"></ratingselect>
          <div class="rating-warp">
            <ul v-if="food.ratings&&food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="(rating,index) in food.ratings" :key="index" class="rating-item border-1px">
                <div class="rating-time">{{rating.rateTime}}</div>
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <div class="user-img">
                    <img :src="rating.avatar" class="avater" alt="">
                  </div>
                </div>

                <p class="text">
                  <i :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></i>
                  {{rating.text}}
                </p>
              </li>
            </ul>
            <div class="no-rating" v-else>

            </div>
          </div>
        </div>

      </div>
    </div>
  </transition>
</template>


<script>
import BScroll from 'better-scroll'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import slpit from '../split/split.vue'
import ratingselect from '../ratingselect/ratingselect.vue'
const ALL = 2
export default {
  props: {
    food: {
      type: Object
    }
  },
  components: {
    cartcontrol,
    slpit,
    ratingselect
  },
  data() {
    return {
      showFoodDetil: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    show() {
      this.showFoodDetil = true
      this.selectType = ALL
      this.onlyContent = true
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.food, {
          click: true
        })
      })
    },
    back() {
      this.showFoodDetil = false
    },
    addFirstFood() {
      this.$set(this.food, 'count', 1)
    },
    selectRating(type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    toggleContent() {
      this.onlyContent = !this.onlyContent
    },
    needShow(type,text) {
        if(this.onlyContent&&!text){
          return false;
        }
        if(this.selectType===ALL){
          return true;
        }else{
          return type===this.selectType
        }
    }
  }
}
</script>

<style lang="less">
@import '../../common/less/mixin.less';
.food {
  position: fixed;
  top: 0;
  bottom: 48px;
  width: 100%;
  z-index: 2;
  background: #fff;
  transform: translate3d(0, 0, 0);
  &.move-enter-active,
  &.move-leave-active {
    transition: all 0.2s linear;
  }
  &.move-enter,
  &.move-leave-active {
    transform: translate3d(100%, 0, 0);
  }
  .img-header {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;
    img {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
    }
    .back {
      position: absolute;
      top: 10px;
      left: 0;
      .icon-arrow_lift {
        display: block;
        padding: 10px;
        font-size: 20px;
        color: #fff;
      }
    }
  }
  .content {
    padding: 18px;
    position: relative;
    .title {
      line-height: 14px;
      margin-bottom: 8px;
      font-size: 14px;
      font-weight: 700;
      color: rgb(7, 17, 27);
    }
    .detail {
      margin-bottom: 18px;
      line-height: 10px;
      .sell-count,
      .rating {
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
    }
    .price {
      font-weight: 700;
      font-weight: 24px;
      .now {
        margin-right: 8px;
        font-size: 14px;
        color: rgb(240, 20, 20);
      }
      .old {
        text-decoration: line-through;
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
    }
    .cartcontrol-wrap {
      position: absolute;
      right: 12px;
      bottom: 12px;
    }
    .buy {
      position: absolute;
      right: 18px;
      bottom: 18px;
      padding: 10px 16px;
      border-radius: 16px;
      font-size: 10px;
      color: #fff;
      background: rgb(0, 160, 220);
      opacity: 1;
      &.fade-enter-active,
      &.fade-leave-active {
        transition: all 0.2s;
      }
      &.fade-enter,
      &.fade-leave-active {
        opacity: 0;
      }
    }
  }
  .info {
    padding: 18px;
    .title {
      margin-bottom: 6px;
      line-height: 14px;
      font-size: 14px;
      color: rgb(7, 17, 27);
    }
    .text {
      padding: 0 8px;
      line-height: 24px;
      font-size: 12px;
      color: #666;
    }
  }
  .rating {
    .title {
      margin-left: 18px;
      line-height: 34px;
      font-size: 14px;
      color: rgb(7, 17, 27);
    }
    .rating-warp {
      padding: 0 18px;
      .rating-item {
        position: relative;
        padding: 16px 0;
        .border-1px;
        .user {
          position: absolute;
          right: 0;
          top: 16px;
          line-height: 12px;
          font-size: 0;
          .name {
            display: inline-block;
            vertical-align: top;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
          .user-img {
            display: inline-block;
            vertical-align: top;
            width: 12px;
            height: 12px;
            img {
              width: 100%;
              border-radius: 50%;
            }
          }
        }
        .rating-time {
          margin-bottom: 6px;
          font-size: 10px;
          line-height: 12px;
          color: rgb(147, 153, 159);
        }
        .text {
          font-size: 12px;
          line-height: 16px;
          color: rgb(7, 17, 27);
          .icon-thumb_up,
          .icon-thumb_down {
            margin-bottom: 4px;
            font-size: 12px;
            line-height: 24px;
          }
          .icon-thumb_up {
            color: rgb(0, 160, 220);
          }
          .icon-thumb_down {
            color: rgb(147, 153, 159);
          }
        }
      }
    }
  }
}
</style>
