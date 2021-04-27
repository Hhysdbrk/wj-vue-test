<template>
  <body id="poster">
  <el-form class="login-container" label-position="left"
           label-width="0px">
    <h3 class="login_title">用户登录</h3>
    <el-form-item>
      <el-input type="text" v-model="loginForm.username"
                auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item>
      <el-input type="password" v-model="loginForm.password"
                auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item style="width: 100%">
      <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
    </el-form-item>
  </el-form>
  </body>
</template>

<script>

export default {
  name: 'Login',
  data() {
    return {
      loginForm: {
        username: 'hhy',
        password: '123'
      },
      responseResult: []
    }
  },
  methods: {
    login() {
      var that = this
      // console.log("route:")
      // console.log(this.$route)
      this.$axios
          .post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          })
          .then(successResponse => {
            // console.log("loginResponse：")
            // console.log(successResponse)
            if (successResponse.data.code === 200) {
              that.$store.commit('login', that.loginForm)
              let path = that.$route.query.redirect
              this.$router.replace({path: path === '/' || path === undefined ? '/index' : path})
            }else {
              this.$alert(successResponse.data.message, 'tip', {
                confirmButtonText: '确定'
              })
            }
          })
          .catch(failResponse => {
          })
    }
  }
}
</script>

<style>
#poster {
  background: url("../assets/img/bg/eva1.jpg") no-repeat;
  background-position: center;
  height: 100%;
  width: 100%;
  background-size: cover;
  position: fixed;
}

body {
  margin: 0px;
}

.login-container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 190px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}

</style>
