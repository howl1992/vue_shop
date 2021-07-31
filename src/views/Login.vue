<template>
  <div class="login_container">
    <div class="login_box">
<!-- 头像区域-->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
<!--  登录表单区域-->
      <el-form  ref="loginFormRef"  class="login_form" :model="loginForm" :rules="loginFormRules">
        <el-form-item prop="username">
          <el-input prefix-icon="el-icon-user" v-model="loginForm.username" placeholder="请输入用户名称"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input prefix-icon="el-icon-unlock" v-model="loginForm.password" type="password" placeholder="请输入用户密码"></el-input>
        </el-form-item>
        <el-form-item class="login_btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>

export default {
  name: 'login',
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          {
            required: true,
            message: '用户名不能为空！',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 18,
            message: '长度在 3 到 18 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: '密码不能为空！',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 18,
            message: '长度在 5 到 18 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
      console.log(this)
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (valid) {
          const { data: res } = await this.$http.post('/login', this.loginForm)

          if (res.meta.status !== 200) return this.$message.error({ message: res.meta.msg })
          this.$message.success('登录成功！')
          window.sessionStorage.setItem('token', res.data.token)
          this.$router.push('/home')
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container{
  background-color: #bbe6d6;
  height: 100%;
}
.login_box{
  width:450px;
  height:300px;
  background-color: #fff;
  border-radius: 3px;
  box-shadow: 0 0 5px black;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  .avatar_box {
    height: 130px;
    width: 130px;
    background-color: #fff;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 5px;
    box-shadow: 0 0 10px #add;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    img {
      width: 100%;
      height:100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }
  .login_btns {
    display: flex;
    justify-content: flex-end;
  }
}

</style>
