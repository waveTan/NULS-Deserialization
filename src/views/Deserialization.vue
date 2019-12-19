<template>
  <div class="deserialization w1200">
    <div class="title">NULS 反序列化</div>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="交易" name="first">
        <div class="fl w_f">
          <el-form :model="transactionForm" :rules="transactionRules" ref="transactionForm" label-width="110px"
                   class="transaction_form">
            <el-form-item label="交易序列化数据" prop="txHex">
              <el-input type="textarea" v-model="transactionForm.txHex"></el-input>
            </el-form-item>
            <el-form-item label="chainId" prop="chainId">
              <el-input v-model="transactionForm.chainId"></el-input>
            </el-form-item>
            <el-form-item label="地址前缀" prop="prdfix">
              <el-input v-model="transactionForm.prdfix"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="success" @click="submitTransaction('transactionForm')">点击反序列化</el-button>
            </el-form-item>
          </el-form>
        </div>
        <div class="w_r fr" v-show="transactionValue">
          <json-viewer
                  :value="transactionValue"
                  :expand-depth="5"
                  copyable
                  boxed
                  sort
          ></json-viewer>
        </div>

      </el-tab-pane>
      <el-tab-pane label="交易业务数据(txData)" name="second">交易业务数据(txData)</el-tab-pane>
      <el-tab-pane label="交易hash查询详情" name="third">根据交易hash查询详情</el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        activeName: 'first',
        transactionForm: {
          txHex: '',
          chainId: '',
          prdfix: '',
        },
        transactionRules: {},

        transactionValue: {},
      };
    },
    created() {
      this.getTransaction();
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      },

      submitTransaction(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },

      getTransaction() {
        let that = this;
        let url = 'http://192.168.1.40:9898/api/jsonrpc';
        let data = [2, '020013d0f95d00008c0117020001f7ec6473df12e751d64cf20a8baa7edd50810f8102000100a029e3110000000000000000000000000000000000000000000000000000000008a1d6e028478cd7300001170200017fe9a685e43b3124e00fd9c8e4e59158baea63450200010000a3e1110000000000000000000000000000000000000000000000000000000000000000000000006b2103958b790c331954ed367d37bac901de5c2f06ac8368b37d7bd6cd5ae143c1d7e348304602210088c184a0ad7448d80d407be677d8221b7bc3bc05eb9263b5dca329205e7213ca022100c04a7aec2179606c41490e076d9f50146e10e4408a12f803ed2ab9b56daca434', 'tNULS'];
        console.log(url,data);
        axios.post(url, data,{headers:{'Content-Type':'application/json'}})
          .then(function (response) {
            console.log(that.transactionValue);
            that.transactionValue = response;
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          });
      },
    }
  }
</script>

<style lang="less">
  .deserialization {
    .title {
      height: 80px;
      font-size: 20px;
      line-height: 80px;
    }
    .w_f {
      width: 500px;
      min-height: 500px;
      margin: 20px 0 100px 0;
      .el-button--success {
        width: 390px;
        text-align: center;
        margin: 0 auto;
      }
    }
    .w_r {
      width: 680px;
      height: 500px;
      border: 1px solid #c0c4cc;
    }
  }
</style>
