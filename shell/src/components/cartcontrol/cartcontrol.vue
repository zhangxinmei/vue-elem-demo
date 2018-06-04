<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="cart-decrease" v-show="food.count>0" v-on:click.stop="decreaseCart">
                <i class="inner icon-remove_circle_outline"></i>
            </div>
        </transition>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add" v-on:click.stop="addCart">
            <i class="icon-add_circle"></i>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            food: {
                type: Object
            }
        },
        created() {
            // console.log(this.food)
        },
        methods: {
            addCart() {
                if (!this.food.count) {
                    this.$set(this.food, 'count', 1)
                } else {
                    this.food.count++;
                }
                this.$emit('add',event.target);
            },
            decreaseCart() {
                if (this.food.count) {
                    this.food.count--;
                }
            }
        }
    }
</script>

<style lang="less">
    .cartcontrol {
        font-size: 0;
        .cart-decrease {
            display: inline-block;
            padding: 6px;
            transition: all 0.4s linear;
            transform: translate3d(0,0,0);
            .inner {
                display: inline-block;
                font-size: 24px;
                color: rgb(0, 160, 220);
                transition: all 0.4s linear;
                transform: rotate(0);
            }
            &.move-enter,&.move-leave{
                opacity: 0;
                transform: translate3d(24px,0,0);
                .inner{
                    transform: rotate(180deg);
                }
            }
        }
        .cart-count {
            display: inline-block;
            width: 12px;
            padding-top: 6px;
            text-align: center;
            vertical-align: top;
            line-height: 24px;
            font-size: 10px;
            color: rgb(147, 153, 159);
        }
        .cart-add {
            display: inline-block;
            padding: 6px;
            .icon-add_circle {
                font-size: 24px;
                color: rgb(0, 160, 220);
            }
        }
    }
</style>

