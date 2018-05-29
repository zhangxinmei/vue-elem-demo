<template>
  <div>
    <div class="goods">
      <div class="menu-wrap" ref="menuWrap">
        <ul>
          <li v-for="(item,index) in goods" :key="index" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
            <span class="text border-1px"> <span class="icon" v-show="item.type>0" :class="classMsp[item.type]"></span>{{item.name}}</span>
          </li>
        </ul>
      </div>
      <div class="foods-wrap" ref="foodsWrap">
        <ul>
          <li v-for="(item,index) in goods" :key="index" class="goods-list" ref="foodList">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li @click="selecFood(food)" v-for="(food,index) in item.foods" :key="index" class="food-item border-1px">
                <div class="item-img">
                  <img :src="food.icon" alt="">
                </div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span class="count">月售{{food.sellCount}}</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartcontrol-wrap">
                    <cartcontrol @add="addFood" :food="food"></cartcontrol>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
    </div>
    <food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script type='text/ecmascript-6'>
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart'
  import cartcontrol from '../cartcontrol/cartcontrol'
  import food from '../food/food'
  const ERR_OK = 0
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      shopcart,
      cartcontrol,
      food
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood:{}
      }
    },
    created() {
      this.classMsp = ["decrease", "discount", "special", "invoice", "guarantee"];
      this.axios.get("/api/goods").then(res => {
        res = res.data;
        if (res.errno === ERR_OK) {
          this.goods = res.data
          console.log(111, this.goods);
          this.$nextTick(() => {
            this.initScroll()
            this.calculateHeight()
            // console.log(this.$refs.foodsWrap.getElementsByClassName("goods-list-hook"))
          })
  
        }
      });
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
  
        }
        return 0
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach(good => {
          good.foods.forEach(food => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods;
      }
    },
    methods: {
      initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrap, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrap, {
          click: true,
          probeType: 3
        })
        this.foodsScroll.on("scroll", (pos) => {
          // 取滑动的正值 Math.abs()
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      calculateHeight() {
        let foodsList = this.$refs.foodList
        let height = 0
        this.listHeight.push(height)
        foodsList.forEach(item => {
          height += item.clientHeight;
          this.listHeight.push(height)
        })
      },
      selectMenu(index, event) {
        console.log(index)
        let foodsList = this.$refs.foodList
        let el = foodsList[index]
        this.foodsScroll.scrollToElement(el, 300)
      },
      addFood(target) {
        this._drop(target)
      },
      _drop(target) {
        // 体验优化，异步执行下滑动画
        this.$nextTick(() => {
          this.$refs.shopcart.drop(target)
        })
  
      },
      selecFood(food) {
        this.selectedFood=food;
        this.$refs.food.show();
      }
    },
  };
</script>

<style lang="less" rel='stylesheet/less'>
  @import '../../common/less/mixin.less';
  .goods {
    display: flex;
    position: absolute;
    top: 176px;
    bottom: 46px;
    width: 100%;
    overflow: hidden;
    .menu-wrap {
      flex: 0 0 80px;
      width: 80px;
      background: #f3f5f7;
      .menu-item {
        display: table;
        height: 54px;
        width: 56px;
        padding: 0 12px;
        line-height: 12px;
        &.current {
          position: relative;
          margin-top: -1px;
          background: #fff;
          z-index: 2;
          font-weight: 700;
          border: none;
        }
        // &::after{
        //   display: inline-block;
        //   height: 100%;
        //   content: '';
        //   vertical-align: middle;
        // }
        .text {
          display: table-cell;
          font-size: 12px;
          vertical-align: middle;
          text-align: center;
          .border-1px;
          .icon {
            display: inline-block;
            vertical-align: top;
            margin-right: 2px;
            width: 12px;
            height: 12px;
            background-size: 12px 12px;
            background-repeat: no-repeat;
            &.decrease {
              background-image: url(./img/decrease_3@2x.png);
            }
            &.discount {
              background-image: url(./img/discount_3@2x.png);
            }
            &.guarantee {
              background-image: url(./img/guarantee_3@2x.png);
            }
            &.invoice {
              background-image: url(./img/invoice_3@2x.png);
            }
            &.special {
              background-image: url(./img/special_3@3x.png);
            }
          }
        }
      }
    }
    .foods-wrap {
      flex: 1;
      .title {
        padding-left: 12px;
        line-height: 26px;
        border-left: 2px solid #d9dde1;
        font-size: 12px;
        color: #93999f;
        background-color: #f3f5f7;
      }
      .food-item {
        display: flex;
        margin: 18px;
        padding-bottom: 18px;
        .border-1px;
        &:last-child {
          .border-none;
          margin-bottom: 0;
        }
        .item-img {
          flex: 0 0 57px;
          margin-right: 10px;
          width: 57px;
          height: 57px;
          img {
            width: 100%;
          }
        }
        .content {
          flex: 1;
          .name {
            margin: 2px 0 8px 0;
            line-height: 14px;
            font-size: 14px;
            color: #07111b;
          }
          .desc,
          .extra {
            line-height: 10px;
            font-size: 10px;
            color: #93999f;
          }
          .desc {
            margin-bottom: 8px;
            line-height: 14px;
          }
          .extra {
            .count {
              margin-right: 12px;
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
            right: 0;
            bottom: 0;
          }
        }
      }
    }
  }
</style>
