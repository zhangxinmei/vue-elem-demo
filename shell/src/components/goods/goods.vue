<template>
  <div class="goods">
    <div class="menu-wrap">
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="menu-item">
          <span class="text"> <span class="icon" v-show="item.type>0" :class="classMsp[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrap"></div>
  </div>
</template>

<script type='text/ecmascript-6'>
const ERR_OK=0
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return{
      goods:[],
    }
  },
  created() {
    this.classMsp = ["decrease", "discount", "special", "invoice", "guarantee"];
    this.axios.get("/api/goods").then(res => {
      res = res.data;
      if (res.errno === ERR_OK) {
        this.goods=res.data
        console.log(111, this.goods);
      }
    });
  }
};
</script>

<style lang="less" rel='stylesheet/less'>
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
    .menu-item{
      display: table;
      height: 54px;
      width: 56px;
      &::after{
        display: inline-block;
        height: 100%;
        content: '';
        vertical-align: middle;
      }
      .text{
        display: inline-block;
        vertical-align: middle;
        font-size: 16px;
      }
    }
  }
  .foods-wrap {
    flex: 1;
  }
}
</style>
