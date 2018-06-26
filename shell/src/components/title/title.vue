<template>
  <div class="title-ct">
    <div class="line"></div>
    <div class="text">{{titleText}}</div>
    <div class="line"></div>
  </div>
</template>

<script>
export default {
  props: ['titleText']
}
</script>

<style lang="less">
.title-ct {
  display: flex;
  width: 80%;
  margin: 28px auto 24px auto;
  .line {
    position: relative;
    flex: 1;
    top: -6px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  }
  .text {
    padding: 0 12px;
    font-weight: 700;
    font-size: 14px;
  }
}
</style>


