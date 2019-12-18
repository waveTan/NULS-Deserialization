<template>
  <div class="account w1200">
    <div class="address">
      <span class="font16">{{$t('public.address')}}: {{accountInfo.address}}</span>
      <font class="fr font16">{{$t('public.balance')}}: {{accountInfo.balance}}<em class="fCN"> NULS</em></font>
    </div>
    <Password ref="password" @passwordSubmit="passSubmit">
    </Password>
  </div>
</template>

<script>
  import Password from '@/components/PasswordBar'
  import {divisionDecimals, connect} from '@/api/util'
  import {getAddressInfoByAddress} from '@/api/requestData'

  export default {
    data() {
      return {
        accountInfo: JSON.parse(localStorage.getItem('accountInfo')),//账户信息
      }
    },
    created() {
      this.addressInfoByAddress(this.accountInfo.address);
    },
    mounted() {
      this.userSetInterval = setInterval(() => {
        this.addressInfoByAddress(this.accountInfo.address);
      }, 10000)
    },
    destroyed() {
      clearInterval(this.userSetInterval);
    },
    components: {
      Password,
    },
    methods: {

      /**
       * @disc:获取地址信息
       * @param address
       * @date: 2019-08-20 16:58
       * @author: Wave
       */
      async addressInfoByAddress(address) {
        let newData = {aesPri: this.accountInfo.aesPri, pub: this.accountInfo.pub};
        let newAddressInfo = await
          getAddressInfoByAddress(address);
        if (newAddressInfo.success) {
          this.accountInfo = {};
          newAddressInfo.data.balance = divisionDecimals(newAddressInfo.data.balance);
          this.accountInfo = {...newData, ...newAddressInfo.data};
        } else {
          this.accountInfo.balance = 0
        }
      },

      /**
       * @disc: url 连接
       * @params: name
       * @params: params
       * @params: type
       * @date: 2019-08-20 18:01
       * @author: Wave
       */
      toUrl(name, parameter, type) {
        let newPath = connect(name, parameter, type);
        if (newPath.success) {
          this.$router.push(newPath.data);
        }
      },
    },
  }
</script>

<style lang="less">
  @import "./../../assets/css/style";

  .account {
    margin: 30px auto 0;
    .address {
      span {
        color: #17202e;
        @media screen and (max-width: 1024px) {
          font-size: 0.8rem;
          display: block;
          margin: 0.5rem 0 0 0.5rem;
        }
      }
      font {
        @media screen and (max-width: 1024px) {
          font-size: 0.8rem;
          display: block;
          float: none;
          margin: 0.5rem 0 0 0.5rem;
        }
      }
    }
  }


</style>
