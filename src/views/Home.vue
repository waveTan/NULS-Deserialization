<template>
  <div class="home">
    <div class="developers">
      <div class="title">
        <h2 class="tc">NULS 开发者工具箱</h2>
        <p class="tl">NULS是一个开源的企业级可定制的区块链平台，为开发者提供快速的区块链解决方案。NULS以微服务、智能合约、跨链互操作性和快速造链为特色，在简化区块链应用实践方面树立了一个新的行业标准。</p>
      </div>
    </div>
    <div>文档</div>
    <div>交流群</div>
  </div>
</template>

<script>
  export default {
    data() {
      return {};
    },
    created() {
    },
    methods: {}
  }
</script>

<style lang="less">
  .home {
    .developers{
      background-image: linear-gradient(to right, #4ff16a 0%, #00da9d 100%);
      width: 100%;
      height: 300px;
      .title{
        width: 500px;
        height: 300px;
        margin: 0 auto;
        h2{
          color: #f0f0f0;
          font-size: 30px;
          line-height: 100px;
        }
        p{
          font-size: 14px;
          line-height: 28px;
          color: #f0f2f5;
        }
      }
  }
  }
</style>
