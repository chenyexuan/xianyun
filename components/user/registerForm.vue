<template>
  <div class="login">
    <el-form :model="form" ref="regForm" :rules="rules" class="form">
      <el-form-item class="form-item" prop="username">
        <el-input placeholder="请输入手机号" v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="captcha">
        <el-input placeholder="请输入验证码" v-model="form.captcha">
          <template slot="append">
            <el-button @click="subCaptcha">发送验证码</el-button>
          </template>
        </el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="nickname">
        <el-input placeholder="你的名字" type="text" v-model="form.nickname"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="password">
        <el-input placeholder="密码" type="password" v-model="form.password"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="checkPassword">
        <el-input placeholder="确认密码" type="password" v-model="form.checkPassword"></el-input>
      </el-form-item>

      <el-button class="submit" type="success" @click="subReg">注册</el-button>
    </el-form>
  </div>
</template>

<script>
import { async } from "q";
export default {
  data() {
    var validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== this.form.password) {
        callback(new Error("两次输入密码不一致!"));
      } else {
        callback();
      }
    };
    return {
      form: {
        username: "",
        nickname: "",
        captcha: "",
        password: "",
        checkPassword: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        nickname: [{ required: true, message: "请输入昵称", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        captcha: [{ required: true, message: "请输入验证码", trigger: "blur" }],
        checkPassword: [{ validator: validatePass2, trigger: "blur" }]
      }
    };
  },
  methods: {
    async subCaptcha() {
      if (!this.form.username) {
        this.$message.warning("请输入手机");
        return;
      } else {
        let res = await this.$axios({
          url: "/captchas",
          method: "post",
          data: { tel: this.form.username }
        });
        // console.log(res)
        if (res.status === 200) {
          this.$message.success("你的验证码为:" + res.data.code);
        } else {
          this.$message.warning("请重新发送验证码!");
        }
      }
    },
    subReg() {
      this.$refs.regForm.validate(async valid => {
        if (valid) {
          const { checkPassword, ...regForm } = this.form;
          // console.log(regForm)
          let res = await this.$axios({
            url: "/accounts/register",
            method: "post",
            data: regForm
          });
        //   console.log(res);
          this.$store.commit("user/setUserInfo", res.data);
          // console.log(this.$store.state)
          this.$message.success("注册成功");
          this.$router.push("/");
        } else {
          return false;
        }
      });
    }
  }
};
</script>

<style lang='less' scoped>
.form {
  padding: 25px;
}

.form-item {
  margin-bottom: 20px;
}

.form-text {
  font-size: 12px;
  color: #409eff;
  text-align: right;
  line-height: 1;
}

.submit {
  width: 100%;
  margin-top: 10px;
}
</style>