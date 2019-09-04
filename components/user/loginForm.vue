<template>
  <div class="login">
    <el-form :model="loginForm" ref="loginForm" :rules="loginRules" class="form">
      <el-form-item class="form-item" prop="username">
        <el-input placeholder="用户名/手机" v-model="loginForm.username"></el-input>
      </el-form-item>
      <el-form-item class="form-item" prop="password">
        <el-input placeholder="密码" type="password" v-model="loginForm.password"></el-input>
      </el-form-item>
      <p class="form-text">
        <nuxt-link to="#">忘记密码</nuxt-link>
      </p>
      <el-button class="submit" type="success" @click="subLogin">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data(){
    return{
      loginForm:{
        username:'',
        password:''
      },
      loginRules:{
        username:[
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password:[
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods:{
    async subLogin(){
      let res = await this.$axios({
        url:'/accounts/login',
        method:'post',
        data:this.loginForm
      })
      console.log(res)
      this.$store.commit('user/setUserInfo',res.data)
      // console.log(this.$store.state)
      this.$message.success('登录成功')
      this.$router.push('/')
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