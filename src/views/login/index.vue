<template>
  <div class="login-container">
    <el-form
      ref="loginForm"
      :model="loginForm"
      :rules="loginRules"
      class="login-form"
      auto-complete="on"
      label-position="left"
    >
      <div class="title-container">
        <h3 class="title">智慧校园</h3>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="Username"
          name="username"
          type="text"
          tabindex="1"
          auto-complete="on"
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
          placeholder="Password"
          name="password"
          tabindex="2"
          auto-complete="on"
          @keyup.enter.native="handleLogin"
        />
        <span class="show-pwd" @click="showPwd">
          <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
        </span>
      </el-form-item>

      <el-button
        :loading="loading"
        type="primary"
        style="width:100%;margin-bottom:30px;"
        @click.native.prevent="handleLogin"
      >登录</el-button>
    </el-form>
    <div class="animation_box">
      <ul>
        <li>胡杰</li>
        <li>代码</li>
        <li>智慧</li>
        <li>使我</li>
        <li>校园</li>
        <li>课程</li>
        <li>快乐</li>
        <li>调查</li>
        <li>个屁</li>
        <li>系统</li>
      </ul>
    </div>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'
import { get, post } from '../../utils/utils.js'
export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('请输入用户名'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('请输入对应密码'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: '杰少',
        password: '666666'
      },
      loginRules: {
        username: [
          { required: true, trigger: 'blur', validator: validateUsername }
        ],
        password: [
          { required: true, trigger: 'blur', validator: validatePassword }
        ]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.toLogin()
          this.$store.dispatch('user/login', this.loginForm)
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    toLogin() {
      post('/user/login', this.loginForm).then(res => {
        if (res.data.code !== 200) {
          this.$message.error(res.data.data.message)
          return
        }
        this.token = res.data.data.token
        this.id = res.data.data.id
        localStorage.setItem('token', res.data.data.token)
        localStorage.setItem('id', res.data.data.id)
        localStorage.setItem('username', res.data.data.username)
        this.getUserInfo(this.id)
      }).catch(err => {
        this.$message.error(err)
      })
    },
    toRegister(values) {
      post('/user/register', values).then((res) => {
        this.onSubmit({ username: this.username2, password: this.password2 })
      }).catch((err) => {
        this.$message.error(err)
      })
    },
    getUserInfo(id) {
      get('/user/info/' + id, { Authorization: this.token }).then(res => {
        this.$store.state.userInfo = res.data.data
        console.log(this.$router)
        this.$router.push({ path: '/lesson_survey' })
        console.log(2)
      }).catch(err => {
        this.$message.error(err)
      })
    }
  }
}
</script>
<style lang="scss">
.animation_box {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  margin: auto;
  z-index: 1;
  width: 100%;
  height: 800px;
  background: linear-gradient(45deg, #feac5e, #c779d0, #4bc0c8);
  ul,
  li {
    list-style: none;
    padding: 0;
    margin: 0;
    color: white;
  }
  li {
    position: absolute;
    bottom: -120px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    color: black;
    background: rgb(255, 255, 255);
  }
  li:nth-child(1) {
    width: 100px;
    height: 100px;
    left: 10%;
    animation: rotate infinite 10s linear 3s;
  }
  li:nth-child(2) {
    width: 80px;
    height: 80px;
    left: 18%;
    animation: rotate infinite 10s linear 7s;
  }
  li:nth-child(3) {
    width: 120px;
    height: 120px;
    left: 27%;
    animation: rotate infinite 10s linear 4s;
  }
  li:nth-child(4) {
    width: 100px;
    height: 100px;
    left: 36%;
    animation: rotate infinite 10s linear 5s;
  }
  li:nth-child(5) {
    width: 100px;
    height: 100px;
    left: 45%;
    animation: rotate infinite 10s linear 6s;
  }
  li:nth-child(6) {
    width: 100px;
    height: 100px;
    left: 54%;
    animation: rotate infinite 10s linear 3s;
  }
  li:nth-child(7) {
    width: 100px;
    height: 100px;
    left: 63%;
    animation: rotate infinite 10s linear 7s;
  }
  li:nth-child(8) {
    width: 100px;
    height: 100px;
    left: 72%;
    animation: rotate infinite 10s linear 2s;
  }
  li:nth-child(9) {
    width: 100px;
    height: 100px;
    left: 81%;
    animation: rotate infinite 10s linear 9s;
  }
  li:nth-child(10) {
    width: 100px;
    height: 100px;
    left: 90%;
    animation: rotate infinite 10s linear 4s;
  }
  @keyframes rotate {
    0% {
      transform: translateY(0px) rotate(-90deg);
      opacity: 0;
    }
    10% {
      transform: translateY(0px) rotate(-90deg);
      opacity: 0;
    }
    30% {
      transform: translateY(-100px) rotate(-20deg);
      opacity: 0.4;
    }
    50% {
      transform: translateY(-240px) rotate(20deg);
      opacity: 0.6;
    }
    80% {
      transform: translateY(-400px) rotate(135deg);
      opacity: 0.4;
    }
    100% {
      transform: translateY(-600px) rotate(270deg);
      opacity: 0;
    }
  }
}
</style>
<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg: #283443;
$light_gray: #fff;
$cursor: #fff;

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
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    color: #454545;
  }
}
</style>

<style lang="scss" scoped>
$bg: #2d3a4b;
$dark_gray: #889aa4;
$light_gray: #eee;

.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  overflow: hidden;

  .login-form {
    position: absolute;
    left: 50%;
    margin-left: -260px;
    z-index: 3;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    overflow: hidden;
  }

  .tips {
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;

    span {
      &:first-of-type {
        margin-right: 16px;
      }
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
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
}
</style>
