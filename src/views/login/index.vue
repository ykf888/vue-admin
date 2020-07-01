<template>
  <div id="login">
    <div class="loginBox">
      <div class="welcome">
        <h1>欢迎登陆</h1>
      </div>
      <div class="fromBox">
        <el-form
          :model="ruleForm"
          :rules="rules"
          ref="ruleForm"
          label-width="0px"
          class="demo-ruleForm"
          :label-position="labelPosition"
        >
          <el-form-item label prop="name">
            <el-input v-model="ruleForm.name" placeholder="请输入邮箱">
              <i slot="prefix" class="iconfont icon-yonghu"></i>
            </el-input>
          </el-form-item>
          <el-form-item label prop="password">
            <el-input
              v-model="ruleForm.password"
              show-password
              minlength="6"
              maxlength="20"
              placeholder="请输入密码"
            >
              <i slot="prefix" class="iconfont icon-mima"></i>
            </el-input>
          </el-form-item>
          <el-form-item label prop="repeatPassword" v-if="isHave">
            <el-input
              v-model="ruleForm.repeatPassword"
              show-password
              minlength="6"
              maxlength="20"
              placeholder="请重复您的密码"
            >
              <i slot="prefix" class="iconfont icon-mima"></i>
            </el-input>
          </el-form-item>
          <el-form-item label prop="captcha" v-if="!isHave">
            <el-row :gutter="20">
              <el-col :span="14">
                <el-input
                  v-model="ruleForm.captcha"
                  minlength="6"
                  maxlength="20"
                  placeholder="请输入验证码"
                >
                  <i slot="prefix" class="iconfont icon-yanzhengma"></i>
                </el-input>
              </el-col>
              <el-col :span="10">
                <el-button type="success" size="medium" style="width:100%">获取验证码</el-button>
              </el-col>
            </el-row>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')" class="loginBotton">{{loginName}}</el-button>
          </el-form-item>
        </el-form>
      </div>
      <div class="noHave" @click="haveAccount">
        <a href="#">{{account}}</a>
      </div>
    </div>
  </div>
</template>
<script>
import {
  stripscript,
  checkEmail,
  checkPassword,
  checkVerification
} from "@/uiils/validate.js";
export default {
  name: "",
  data() {
    // 验证验证码
    var captcha = (rule, value, callback) => {
      this.ruleForm.captcha = stripscript(value);
      value = this.ruleForm.captcha;
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else if (!checkVerification(value)) {
        callback(new Error("验证码有误"));
      } else {
        callback();
      }
    };
    // 验证邮箱
    var name = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入邮箱"));
      } else if (!checkEmail(value)) {
        callback(new Error("邮箱格式有误"));
      } else {
        callback();
      }
    };
    // 验证密码

    var password = (rule, value, callback) => {
      this.ruleForm.password = stripscript(value);
      value = this.ruleForm.password;
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (!checkPassword(value)) {
        callback(new Error("密码为6～20位数字+字母"));
      } else {
        callback();
      }
    };
    // 验证再次输入密码
    var repeatPassword = (rule, value, callback) => {
      this.ruleForm.repeatPassword = stripscript(value);
      value = this.ruleForm.repeatPassword;
      if (value !== this.ruleForm.password) {
        callback(new Error("两次输入的密码不一致"));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        name: "",
        password: "",
        captcha: "",
        repeatPassword: ""
      },
      labelPosition: "left",
      account: "还没账号？立即注册",
      loginName:'立即登陆',
      isHave:false,
      rules: {
        name: [{ validator: name, trigger: "blur" }],
        password: [{ validator: password, trigger: "blur" }],
        repeatPassword: [{ validator: repeatPassword, trigger: "blur" }],
        captcha: [{ validator: captcha, trigger: "blur" }]
      }
    };
  },
  mouted() {},
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("成功!");
        } else {
          console.log("error submit!!");
          return false;
        }
   
      });
    },
    haveAccount() {
      this.ruleForm={
        name: "",
        password: "",
        captcha: "",
        repeatPassword: ""
      }
      if (this.isHave===false) {
        this.account = "已有账号？立即登陆"
        this.isHave=true
        this.loginName="立即注册"
      }else{
        this.account = "还没账号？立即注册"
        this.isHave=false
        this.loginName="立即登陆"
      }
      
    }
  }
};
</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  // background-color: #344a5f;
  display: flex;
  align-items: center;
  justify-content: center;
  background: url(/images/bg.jpg) center center no-repeat;
  background-size: cover;
  z-index: -1;
  .welcome {
    text-align: center;
    margin-top: 10px;
    h1 {
      font-size: 20px;
      margin: 10px 0;
      margin-bottom: 20px;
    }
  }
  .noHave {
    text-align: center;
    margin-top: 10px;
    a {
      font-size: 14px;
      color: #999;
      &:hover{
        color: #227ce4;
        text-decoration: underline;
      }
    }
  }
  .fromBox {
    padding: 0 20px;
  }
  .loginBox {
    width: 350px;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    .topButton {
      display: flex;
      align-items: center;
      justify-content: center;
      span {
        display: block;
        padding: 8px 20px;
        border-radius: 4px;
        color: #999;
        font-size: 14px;
        cursor: pointer;
      }
      .activeStyle {
        background-color: #2f4255;
      }
    }
  }
  .loginBotton {
    width: 100%;
    margin-top: 20px;
    border-radius: 99px;
    text-align: center;
    box-shadow: 0px 4px 6px rgba(72, 152, 242, 0.5);
  }
}
</style>