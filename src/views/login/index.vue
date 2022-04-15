<template>
  <div class="login-container">      
   <div class="login-form" v-loading="isloading">
      <div class="login-img">
      </div>
      <div class="login-text">
        <el-form
          ref="loginForm"
          :model="loginForm"
          :rules="loginRules"
          auto-complete="on"
          label-position="left"
        >
          <div class="title-container">
            <h3 class="title">
              <svg-icon icon-class="wlogo" />
              登录系统
            </h3>
          </div>

          <el-form-item prop="username">
            <span class="svg-container">
              <svg-icon icon-class="user" />
            </span>
            <el-input
              ref="username"
              v-model="loginForm.username"
              placeholder="请输入用户名"
              name="username"
              type="text"
              tabindex="1"
              auto-complete="on"
              @keyup.enter.native="handLoginBtn"
            />
          </el-form-item>

          <el-form-item prop="password">
            <span class="svg-container">
              <svg-icon icon-class="password" />
            </span>
            <el-input
              :key="passwordType"
              ref="password"
              v-model="loginForm.password"
              :type="passwordType"
              placeholder="请输入密码"
              name="password"
              tabindex="2"
              auto-complete="off"
              @keyup.enter.native="handLoginBtn"
            />
            <span class="show-pwd" @click="showPwd">
              <svg-icon
                :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'"
              />
            </span>
          </el-form-item>

          <el-button
            :loading="loading"
            type="primary"
            style="width: 100%; margin-bottom: 30px"
          @keyup.enter.native="handLoginBtn"
             @click="handLoginBtn"
            >登录</el-button
          >            
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import { validUsername } from "@/utils/validate";

export default {
  name: "Login",
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error("Please enter the correct user name"));
      } else {
        callback();
      }
    };
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("The password can not be less than 6 digits"));
      } else {
        callback();
      }
    };
    return {
       dialogVisible: false,
      loginForm: {
        username: "",
        password: "",
      },
      loginRules: {
        username: [
          { required: true, message: '请输入账户', trigger: 'blur' },
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
        ],
      },
      loading: false,
      passwordType: "password",
      redirect: undefined,
      appList:[

      ],
      isloading:false,
      ispublic:false
    };
  },
  watch: {
    $route: {
      handler: function (route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true,
    },
  },
   mounted() {              
      // this.loadapp();
      this.initAccount();
  },
  methods: {
    showPwd() {
      if (this.passwordType === "password") {
        this.passwordType = "";
      } else {
        this.passwordType = "password";
      }
      this.$nextTick(() => {
        this.$refs.password.focus();
      });
    },
   handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({ path: this.redirect || '/' })
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    handLoginBtn(){
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
            this.handleLogin();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },   
    handleClose(){
      this.dialogVisible=false;
    },
    initAccount(){
      process.env.NODE_ENV === 'development' &&(this.loginForm.username="admin",this.loginForm.password="111111");
    }
  },
};
</script>


<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg: #283443;
$light_gray: #999; //fff
$cursor: #333;
@import "~@/styles/variables.scss";
@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        // box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid $menuActiveText; //rgba(255, 255, 255, 0.1)
    //background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg: #eff2fd; //2d3a4b
$dark_gray: #889aa4;
$light_gray: #333; //eee
@import "~@/styles/variables.scss";
.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 980px;
    max-width: 100%;
    margin: 0 auto;
    overflow: hidden;
    margin-top: 10%;
    background: #fff;
    display: flex;
    box-shadow: 0 0 8px #d5d5d9;
    border-radius: 2px;
    .login-img {
      flex: 1;
      flex-shrink: 0;
      background: url(~@/assets/img/ogoimg.png) no-repeat left center transparent;background-size: auto 100%;
    }
    .login-text {
      flex-basis: 420px;
      width: 490px;
      padding: 80px 50px 80px 40px;
    }
  }

  .tips {
    font-size: 14px;
    color: #999;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $menuActiveText; // $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;
    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
  .wsystem{
    padding:60px 30px 80px;
      display: grid;
      gap: 20px;
       grid-template-columns: repeat(auto-fill,30%);
       justify-content: center;
       justify-items: center;
    .wsystem-list{
        border: 1px solid #f2f2f9;
        text-align: center;
        flex-shrink: 0;
        min-height: 200px;
        width: 100%;
      &:hover{
        border: 1px solid #557ff7;
      }      
          .wsystem-img{
            font-size: 66px;
          }
          .wsystem-text{
            font-size: 20px;
            margin-top: 10px;
         
      }
     
    }
  }
}
</style>
