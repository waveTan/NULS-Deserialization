<template>
  <div class="deserialization w1200">
    <div>NULS 反序列化</div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="交易" name="first">交易</el-tab-pane>
      <el-tab-pane label="交易业务数据(txData)" name="second">交易业务数据(txData)</el-tab-pane>
      <el-tab-pane label="交易hash查询详情" name="third">根据交易hash查询详情</el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        activeName: 'second'
      };
    },
    created() {
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      },
    }
  }
</script>

<style lang="less">
  .home {

  }
</style>
