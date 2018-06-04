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
            </div>
        </div>
    </transition>
</template>


<script>
    import BScroll from 'better-scroll'
    import cartcontrol from '../cartcontrol/cartcontrol.vue'
    export default {
        props: {
            food: {
                type: Object
            }
        },
        components: {
            cartcontrol
        },
        data() {
            return {
                showFoodDetil: false
            }
        },
        methods: {
            show() {
                this.showFoodDetil = true;
                this.$nextTick(() => {
                    this.scroll = new BScroll(this.$refs.food, {
                        click: true
                    })
                })
            },
            back() {
                this.showFoodDetil = false;
            },
            addFirstFood() {
                this.$set(this.food, 'count', 1);
            }
        }
    }
</script>

<style lang="less">
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
                &.fade-enter-active,&.fade-leave-active{
                    transition: all 0.2s;
                }
                &.fade-enter,&.fade-leave-active{
                    opacity: 0;

                }
            }
        }
    }
</style>
