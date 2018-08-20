<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tabs border-1px">
      <div class="tabs-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tabs-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tabs-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from "./components/header/header";
import axios from "axios";
const ERR_OK = 0;
export default {
  name: "App",
  components: {
    "v-header": header
  },
  data() {
    return {
      seller: {}
    };
  },
  created() {
    this.axios.get("/api/seller").then(res => {
      res = res.data;
      if (res.errno === ERR_OK) {
        this.seller = res.data;
      }
    });
    this.getAjax("/api/seller").then((res)=>{
      console.log(444,res)
    })
   
    // this.axiosTest("/api/seller")
    //   .then(res => {
    //     this.axiosTest("/api/ratings");
    //   })
    //   .then(res => {
    //     // console.log("ratings", res);
    //   });
    // this.getTest();
  },
  methods: {
    // async axiosTest(url) {
    //   return this.axios.get(url);
    // },
    // async getTest() {
    //   const sellerRes = await this.axiosTest("/api/seller");
    //   const ratingRes = await this.axiosTest("/api/ratings");
    //   const goodRes = await this.axiosTest("/api/goods");
    // }
    getAjax(url, params) {
      return new Promise((resolve, reject) => {
        axios
          .get(url, {
            params: params
          })
          .then(res => {
            resolve(res.data);
          })
          .catch(err => {
            reject(err.data);
          });
      });
    },
  }
};
</script>

<style lang="less">
@import "./common/less/mixin.less";
.tabs {
  display: flex;
  width: 100%;
  height: 40px;
  line-height: 40px;
  .border-1px;
  .tabs-item {
    text-align: center;
    flex: 1;
    a {
      display: block;
      font-size: 14px;
      color: rgb(77, 85, 93);
      &.active {
        color: rgb(240, 20, 20);
      }
    }
  }
}
</style>

