<template>
    <transition name="move">
        <div class="food" v-show="showFoodDetil">
           <div class="food-content">
               <div class="img-header">
                   <img :src="food.image" alt="">
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
        transform: translate3d(0,0,0);
        &.move-enter-active, &.move-leave-active{
            transition: all 0.2s linear;
        }
        &.move-enter,&.move-leave-active{
            transform: translate3d(100%,0,0);
        }
    }
</style>
