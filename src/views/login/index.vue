<template>
  <div id="login">
    <div class="loginBox">
      <div class="topButton">
        <span
          :class="buttomName===item.name?'activeStyle':''"
          :key="item.id"
          v-for="item in bottonName"
          @click="activeStyle(item)"
        >{{item.name}}</span>
      </div>
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="demo-ruleForm"
        :label-position="labelPosition"
      >
        <el-form-item label="账号" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="ruleForm.password" show-password></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  name: "",
  data() {
    return {
      bottonName: [
        {
          name: "登陆"
        },
        {
          name: "注册"
        }
      ],
      buttomName: "登陆",
      ruleForm: {
        name: "",
        password: ""
      },
      labelPosition: "top",
      rules: {
        name: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入活动名称", trigger: "blur" },
          { min: 3, max: 5, message: "长度在 3 到 5 个字符", trigger: "blur" }
        ]
      }
    };
  },
  mouted() {},
  methods: {
    activeStyle(item) {
      if (this.buttomName === item.name) {
        buttomName = "";
      } else {
        this.buttomName = item.name;
      }
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>
<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
  display: flex;
  align-items: center;
  justify-content: center;
  .loginBox {
    width: 350px;
    height: 410px;

    .topButton {
      display: flex;
      align-items: center;
      justify-content: center;
      span {
        display: block;
        padding: 8px 20px;
        border-radius: 4px;
        color: #fff;
        font-size: 14px;
        cursor: pointer;
      }
      .activeStyle {
        background-color: #2f4255;
      }
    }
  }
}
</style>