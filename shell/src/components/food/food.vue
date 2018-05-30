<template>
    <transition name="move">
        <div class="food" v-show="showFoodDetil">
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
                        <span class="old">￥{{food.oldPrice}}</span>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>


<script>
    export default {
        props: {
            food: {
                type: Object
            }
        },
        data() {
            return {
                showFoodDetil: false
            }
        },
        methods: {
            show() {
                this.showFoodDetil = true;
            },
            back(){
                this.showFoodDetil = false;
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
            .back{
                position: absolute;
                top: 10px;
                left: 0;
                .icon-arrow_lift{
                    display: block;
                    padding: 10px;
                    font-size: 20px;
                    color: #fff;
                }
            }
        }
    }
</style>
