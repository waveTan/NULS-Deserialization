<template>
  <div class="header bg-white">
    <div class="w1200">
      <div class="logo fl">
        <img class="clicks" @click="toUrl('home')" :src="logoSvg"/>
      </div>
      <div class="nav fl">
        <el-menu :default-active="activeIndex" class="fl" mode="horizontal" @select="handleSelect">
          <el-menu-item index="deserialization">{{$t('nav.selectItem')}}</el-menu-item>
          <!--<el-menu-item index="page2">{{$t('nav.issueToken')}}</el-menu-item>-->
        </el-menu>
        <div class="user fr tc">
          <div class="height fl">
            <Height></Height>
          </div>
          <div class="fr">
            <!--<div class="user_info fl" v-if="accountAddress">
              <el-menu mode="horizontal" @select="handleSelect" class="user_menu">
                <el-submenu index="user">
                  <template slot="title"><i class="el-icon-s-custom click "></i>&nbsp;
                  </template>
                  <el-menu-item index="userInfo">{{$t('nav.user')}}</el-menu-item>
                  <el-menu-item index="backupsAddress">{{$t('nav.backup')}}</el-menu-item>
                  <el-menu-item index="signOut">{{$t('nav.signOut')}}</el-menu-item>
                </el-submenu>
              </el-menu>
            </div>
            <div v-if="!accountAddress" class="click fl landing sub_info" @click="toUrl('newAddress')">
              {{$t('nav.login')}}
            </div>-->
            <div class="language fr click sub_info" @click="selectLanguage">{{lang === 'en' ? '简体中文':'English' }}</div>
          </div>
        </div>
      </div>
      <div class="nav_mobile">
        <div class="fr mr_20">
          <i class="el-icon-menu font24" @click="showMenu"></i>
        </div>
        <transition name="el-zoom-in-center">
          <div v-show="isMenu" @click="hideMenu">
            <el-menu :default-active="activeIndex" class="fl" mode="vertical" @select="handleSelect">
              <el-menu-item index="page1">{{$t('nav.selectItem')}}</el-menu-item>
              <el-menu-item index="page2">{{$t('nav.issueToken')}}</el-menu-item>
              <el-submenu index="user" v-if="accountAddress">
                <template slot="title"><i class="el-icon-s-custom click "></i>&nbsp;
                </template>
                <el-menu-item index="userInfo">{{$t('nav.user')}}</el-menu-item>
                <el-menu-item index="backupsAddress">{{$t('nav.backup')}}</el-menu-item>
                <el-menu-item index="signOut">{{$t('nav.signOut')}}</el-menu-item>
              </el-submenu>
              <el-menu-item v-if="!accountAddress" class="click font14 landing" @click="toUrl('newAddress')">
                {{$t('nav.login')}}
              </el-menu-item>
              <el-menu-item class="language font14 click" @click="selectLanguage">{{lang === 'en' ? '简体中文':'English' }}
              </el-menu-item>
            </el-menu>
          </div>
        </transition>

      </div>
    </div>
    <Password ref="password" @passwordSubmit="passSubmit"></Password>
    <div class="cb"></div>
  </div>
</template>

<script>
  import {RUN_DEV} from '@/config'
  import logoBeta from '@/assets/img/logo-beta.svg'
  import logo from '@/assets/img/logo.svg'
  import Height from '@/components/Height'
  import Password from '@/components/PasswordBar'
  import {passwordVerification} from '@/api/util'

  export default {
    data() {
      return {
        logoSvg: RUN_DEV ? logo : logoBeta,
        activeIndex: '1',//导航选中
        accountInfo: {},//账户信息
        accountAddress: '',//账户地址
        lang: 'en',
        isMenu: true,//手机版菜单显示及隐藏
      };
    },
    components: {Height, Password},
    created() {
    },
    mounted() {
      setInterval(() => {
        if (localStorage.hasOwnProperty('accountInfo')) {
          this.accountInfo = JSON.parse(localStorage.getItem('accountInfo'));
          this.accountAddress = this.accountInfo.address;
        }
      }, 500);
    },
    methods: {

      /**
       * 语言切换
       */
      selectLanguage() {
        this.lang = this.lang === 'en' ? 'cn' : 'en';
        sessionStorage.setItem('lang', this.lang);
        this.$i18n.locale = this.lang;
      },

      /**
       * 导航切换
       * @param key
       */
      handleSelect(key) {
        if (key === 'deserialization') {
          this.toUrl('deserialization')
        } else if (key === 'page2') {
          this.toUrl('page2')
        } else if (key === 'userInfo') {
          this.toUrl('user')
        } else if (key === 'backupsAddress') {
          this.toUrl('backupsAddress');
        } else if (key === 'signOut') {
          this.signOut();
        }
      },

      /**
       * 退出
       */
      signOut() {
        this.$confirm(this.$t('tips.tips12'), this.$t('tips.tips11'), {
          confirmButtonText: this.$t('backupsAddress.backupsAddress41'),
          cancelButtonText: this.$t('nav.signOut'),
          type: 'warning',
          showClose: false,
          closeOnClickModal: false,
          closeOnPressEscape: false,
          center: true
        }).then(() => {
          this.toUrl('backupsAddress');
        }).catch(() => {
          this.$refs.password.showPassword(true);
        });
      },

      /**
       *  获取密码框的密码
       * @param password
       **/
      async passSubmit(password) {
        let isPassword = await passwordVerification(this.accountInfo, password);
        if (isPassword.success) {
          localStorage.removeItem('accountInfo');
          sessionStorage.removeItem('data');
          this.accountInfo = {};
          this.accountAddress = '';
          this.toUrl('newAddress')
        } else {
          this.$message({message: this.$t('tips.tips4'), type: 'error', duration: 3000});
        }
      },

      /**
       * @disc: 手机版菜单显示及隐藏
       * @params:
       * @date: 2019-10-11 9:54
       * @author: Wave
       */
      showMenu() {
        this.isMenu = !this.isMenu;
      },

      /**
       * @disc: 手机版菜单隐藏
       * @date: 2019-10-11 11:12
       * @author: Wave
       */
      hideMenu() {
        this.isMenu = false;
      },

      /**
       * url 连接
       * @param name
       */
      toUrl(name) {
        this.$router.push({
          name: name,
        })
      }
    }
  }
</script>

<style lang="less">
  @import "./../assets/css/style";

  .header {
    border-bottom: 1px solid #bebebe;
    height: 100px;
    line-height: 100px;
    @media screen and (max-width: 1024px) {
      height: 60px;
      line-height: 60px;
    }
    .logo {
      width: 104px;
      margin: 18px 0 0 0;
      height: 42px;
      @media screen and (max-width: 1024px) {
        margin: 10px 0 0 5px;
        height: 40px;
      }
      img {
        width: 100px;
        @media screen and (max-width: 1024px) {
          width: 80px;
        }
      }
    }
    .nav {
      width: 1095px;
      height: 98px;
      @media screen and (max-width: 1200px) {
        width: 920px;
      }
      @media screen and (max-width: 1024px) {
        display: none;
      }
      .el-menu.el-menu--horizontal {
        border-bottom: 0;
        width: 850px;
        height: 99px;
        @media screen and (max-width: 1200px) {
          width: 680px;
        }
        .el-menu-item {
          height: 99px;
          line-height: 100px;
          padding: 0;
          margin: 0 25px;
          font-size: 18px;
          color: #333;
          &:hover {
            color: @Ncolour;
          }
          &:first-child {
            margin-left: 75px;
          }
        }
        .is-active {
          color: @Ncolour;
          border-bottom: 0 solid transparent;
        }
      }
      .user {
        .height {
          float: left;
        }
        .user_info {
          width: 28px;
          .user_menu {
            width: 28px;
            text-align: center;
            .el-submenu {
              &:hover {
                background-color: transparent;
              }
              .el-submenu__title {
                line-height: 20px;
                height: 20px;
                margin: 40px auto 0;
                padding: 0;
                width: 25px;
                border: 0;
                &:hover {
                  background-color: transparent;
                }
                .el-icon-arrow-down {
                  font-size: 0;
                }
              }
            }
          }
        }
        .language {
          width: 70px;
          line-height: 99px;
          margin-left: 20px;
        }
      }
      .landing {
        width: 30px;
        line-height: 99px;
        text-align: center;
        z-index: 99;
        position: relative;
      }
    }
    .nav_mobile {
      display: none;
      @media screen and (max-width: 1024px) {
        z-index: 100000;
        display: block;
        position: absolute;
        right: 0;
        line-height: 40px;
        .el-icon-menu {
          margin: 15px 0 0 0;
        }

        .el-menu {
          border: 0;
          float: right;
          z-index: 100001;
          margin: 10px 0 0 0;
        }

        .landing {
          width: auto;
          line-height: 56px;
        }

        .language {
          width: auto;
          line-height: 56px;
        }
      }
    }
  }

  .el-message-box__wrapper {
    .el-message-box__content {
      .el-message-box__message {
        p {
          color: red;
        }
      }
    }
  }
</style>
