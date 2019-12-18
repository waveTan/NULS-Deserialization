<template>
  <div class="block_height">
    <i class="el-icon-s-grid fCN"></i>
    <span> {{height}} </span>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        height: 0,//最新高度
      };
    },
    created() {
      this.getBestBlockHeader();
    },
    mounted() {
      setInterval(() => {
        this.getBestBlockHeader()
      }, 10000)
    },
    methods: {

      /**
       * 获取最新高度
       */
      getBestBlockHeader() {
        this.$post('/', 'getBestBlockHeader', [])
          .then((response) => {
            //console.log(response)
            if (response.hasOwnProperty("result")) {
              this.height = response.result.height;
            } else {
              this.height = 0;
            }
          }).catch((error) => {
          this.height = 0;
          console.log(error);
        })
      },
    }
  }
</script>

<style lang="less" type="text/less">
  .block_height {
    width: 120px;
    line-height: 99px;
  }
</style>
