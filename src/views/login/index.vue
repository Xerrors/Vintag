<template>
  <div class="login-container" :style="{ backgroundImage: 'url(' +bg + ')'}">
    <div class="inner"/>
    <div class="sub-container">
      <div class="login-form">
        <el-form
          ref="loginForm"
          :model="loginForm"
          :rules="loginRules"
          auto-complete="on"
          label-position="left"
        >
          <!-- 标题 -->
          <p class="title en">LOG IN</p>
          <p class="title cn">请登录</p>
          <!-- 用户名 -->
          <el-form-item prop="username" class="el-form-login">
            <el-input
              v-model.trim="loginForm.username"
              name="username"
              type="text"
              auto-complete="off"
              placeholder="USERNAME OR E-MAIL"
            />
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password" class="el-form-login">
            <el-input
              :type="pwdType"
              v-model.trim="loginForm.password"
              name="password"
              auto-complete="off"
              placeholder="PASSWORD"
              @keyup.enter.native="handleLogin"
            />
            <span class="show-pwd" @click="showPwd">
              <svg-icon :icon-class="eyeState"/>
            </span>
          </el-form-item>
          <!-- 登陆按钮 -->
          <div class="forgot-container">
            <div class="login-btn" v-loading="loading" role="button" @click="handleLogin">
              <p>登录</p>
            </div>
            <div class="forgot-passwd" @click="forgotPasswd">
              <p>忘记密码？</p>
              <p>Fotgot Password?</p>
            </div>
          </div>
        </el-form>
      </div>
      <div class="login-methods">
        <p class="title">使用其它方式登录</p>
        <div class="method-container">
          <span class="svg-container">
            <svg-icon icon-class="qq"/>
          </span>
          <a class="login-method-btn" @click="qqLogin">QQ账户登录</a>
        </div>
        <div class="method-container">
          <span class="svg-container">
            <svg-icon icon-class="google"/>
          </span>
          <a class="login-method-btn" @click="googleLogin">Google账户登录</a>
        </div>
        <div class="method-container">
          <span class="svg-container">
            <svg-icon icon-class="qrcode"/>
          </span>
          <a class="login-method-btn" @click="QRcodeLogin">关联APP扫码登录</a>
        </div>
      </div>
      <div class="register-container">
        <a class="register-btn" @click="register">还没有账户？ 立即注册吧！</a>
        <span class="svg-container">
          <svg-icon icon-class="right"/>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import "@/styles/font.scss";
import axios from "axios";
export default {
  name: "Login",
  data() {
    return {
      bg: require("../../assets/background/login-bg1.jpg"),
      eyeState: "eye-close",
      loginForm: {
        username: "",
        password: ""
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", message: "用户名不能为空" }
        ],
        password: [{ required: true, trigger: "blur", message: "密码不能为空" }]
      },
      loading: false,
      pwdType: "password",
      redirect: undefined
    };
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect;
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.pwdType === "password") {
        this.pwdType = "";
        this.eyeState = "eye-open";
      } else {
        this.eyeState = "eye-close";
        this.pwdType = "password";
      }
    },
    // 执行用户登录操作
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true;
          this.$store
            .dispatch("Login", this.loginForm)
            .then(() => {
              this.loading = false;
              this.$router.push({ path: this.redirect || "/" });
            })
            .catch(err => {
              this.loading = false;
            });
        } else {
          this.$message.error("用户信息格式错误！");
          return false;
        }
      });
    },
    forgotPasswd() {
      this.$alert("要不试试 账户：admin，密码：admin", "Oooops", {
        confirmButtonText: "确定"
      });
    },
    qqLogin() {
      this.$message("功能开发中！");
    },
    googleLogin() {
      this.$message("功能开发中！");
    },
    QRcodeLogin() {
      this.$message("功能开发中！");
    },
    register() {
      // this.$emit("changeLoginState");
      // this.$message("功能开发中！");
      this.$router.push("/register");
    }
  }
};
</script>

<style rel="stylesheet/scss" lang="scss">
$font_dark: #666;
$font_light: rgb(255, 255, 255);
$box_gray: rgb(191, 191, 191);
$place-holder: rgb(77, 76, 76);
/* reset element-ui css */
.login-container {
  .el-input {
    display: inline-block;
    height: 66px;
    width: 85%;
    input {
      background-color: $box_gray;
      border: 0px;
      -webkit-appearance: none;
      border-radius: 10px;
      font-size: 16px;
      padding: 12px 60px 12px 60px;
      color: #000;
      height: 66px;
      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px #000 inset !important;
        -webkit-text-fill-color: #000 !important;
      }
    }
    input::-webkit-input-placeholder,
    textarea::-webkit-input-placeholder {
      color: $place-holder;
      font-size: 16px;
    }
  }
}
@media (max-width: 1250px) {
  .login-container {
    .el-input {
      height: 66px;
      input {
        font-size: 14px;
        height: 50px;
      }
      input::-webkit-input-placeholder,
      textarea::-webkit-input-placeholder {
        color: $place-holder;
        font-size: 12px;
      }
    }
  }
}
</style>

<style lang="scss">
@media (max-width: 1250px) {
  .el-form-login {
    .el-form-item__error {
      position: absolute;
      top: 100%;
      left: 50px !important;
    }
  }
}
</style>

<style rel="stylesheet/scss" lang="scss" scoped>
$bg: url("../../assets/background/login-bg1.jpg");
$dark_gray: #889aa4;
$theme_blue: rgb(52, 128, 255);
$font_light: rgb(255, 255, 255);
.login-container {
  position: fixed;
  width: 100%;
  height: 100%;
  background-size: 100% 100%;
  .inner {
    width: 100%;
    height: 100%;
    background-color: rgba(18, 18, 18, 0.5);
  }
  // 子容器
  .sub-container {
    width: 80%;
    max-width: 1000px;
    min-width: 400px;
    height: 500px;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    // 登录表单
    .login-form {
      min-width: 400px;
      height: 423px;
      .title {
        font-family: "SourceHanSerifCN";
        color: $font_light;
        opacity: 0;
        animation: slideUp 1s forwards;
      }
      .cn {
        margin-top: 5px;
      }
      .en {
        margin-bottom: 5px;
      }
      .el-form-item {
        position: relative;
        margin-bottom: 30px;
        opacity: 0;
        animation: slideUp 1s 0.2s forwards;
      }
      .show-pwd {
        position: absolute;
        width: 40px;
        right: 120px;
        margin-top: auto;
        margin-bottom: auto;
        @media (min-width: 1250px) {
          top: 10px;
        }
        @media (max-width: 1250px) {
          right: 60px;
        }
        top: 5px;
        font-size: 30px;
        color: $dark_gray;
        cursor: pointer;
      }
      .login-btn {
        background-color: $theme_blue;
        width: 117px;
        height: 43px;
        @media (max-width: 1250px) {
          width: 100px;
          height: 38px;
        }
        p {
          margin: 0 0;
          position: relative;
          text-align: center;
          font-size: 17px;
          font-family: "SourceHanSansSC";
          color: rgb(255, 255, 255);
          line-height: 43px;
          @media (max-width: 1250px) {
            font-size: 14px;
            line-height: 38px;
          }
        }
        cursor: pointer;
        transition: box-shadow 0.2s ease-out;
        box-shadow: 0px 0px 0px #ba5d18;
      }
      .login-btn:hover {
        box-shadow: 3px 3px 0px #ba5d18;
      }
      .login-btn p:hover {
        animation: moveup 0.3s ease-out;
      }
      .forgot-container {
        margin-top: 48px;
        display: flex;

        opacity: 0;
        animation: slideUp 1s 0.4s forwards;
      }
      .forgot-passwd {
        :first-child {
          margin-bottom: 0px;
        }
        :last-child {
          margin-top: 0px;
        }
        margin-left: 35px;
        p {
          height: 12px;
          font-size: 10px;
          font-style: italic;
          font-family: "SourceHanSansSC";
          color: $font-light;
          cursor: pointer;
          @media (max-width: 1250px) {
            font-size: 10px;
          }
        }
        &:hover {
          p {
            color: $theme_blue;
          }
        }
      }
    }
    // 登录方式
    .login-methods {
      width: 40%;
      min-width: 400px;
      height: 423px;
      opacity: 0;
      animation: slideUp 1s forwards;
      .title {
        font-size: 29px;
        font-family: "SourceHanSerifCN";
        color: $font_light;
        line-height: 1.621;
        margin: 40px 60px 40px 60px;
      }
      .method-container {
        display: flex;
        margin-left: 80px;
        margin-top: 40px;
        .svg-container {
          width: 43px;
          font-size: 43px;
          color: $theme_blue;
        }
        .login-method-btn {
          margin-left: 35px;
          font-size: 15px;
          font-family: "SourceHanSansSC";
          color: $font_light;
          line-height: 3.133;
          &:hover {
            color: $theme_blue;
          }
        }
      }
    }
    .register-container {
      margin-top: 90px;
      font-style: italic;
      font-size: 15px;
      font-family: "SourceHanSansSC";
      color: $theme_blue;
      line-height: 3.133;
      text-shadow: 0px 0px 8.46px rgba(129, 129, 129, 0.004);
      a {
        border-bottom-color: $theme_blue;
        border-bottom-width: 1px;
      }
      opacity: 0;
      animation: slideUp 1s 0.5s forwards;
      text-align: center;
    }
  }
}
@media (max-width: 1250px) {
  .el-form-item {
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    text-align: center;
  }
  .login-form {
    width: 100%;
    max-width: 400px;
    margin: 0 auto;
    .title {
      margin: 20px 0px;
      text-align: center;
      line-height: 1;
    }
    .cn {
      font-size: 33px;
      margin-bottom: 40px;
    }
    .en {
      font-size: 27px;
    }
    .el-input {
      height: 66px;
      width: 80%;
      display: inline-block;
      input {
        font-size: 16px;
      }
    }
    .forgot-container {
      width: 80%;
      margin: 0 auto;
    }
  }
  .login-methods {
    display: none;
  }
}
@media (min-width: 1250px) {
  .sub-container {
    // flex 布局
    display: -webkit-flex; /* Safari */
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
  }
  .login-form {
    width: 60%;
    border: 0px solid;
    border-right-color: rgb(191, 191, 191);
    border-right-width: 1px;
    .title {
      font-size: 38px;
      line-height: 1.237;
    }
  }
}
@keyframes moveup {
  0% {
    top: 0;
  }
  25% {
    top: -15px;
  }
  50% {
    top: 0;
  }
  75% {
    top: 15px;
  }
  100% {
    top: 0;
  }
}
@keyframes slideUp {
  0% {
    transform: translateY(100px);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
