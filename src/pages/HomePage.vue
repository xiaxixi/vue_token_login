<template>
  <div>
    <el-button type="primary" @click="toLogin" target="_balnk">登陆</el-button>

    <div style="margin:10px">
      token用户登录注册
      <span>前端：vue2.5 + element</span>
      <span>服务端：koa2</span>
      <span>数据库：mongoDB</span>
      <span>http安全通讯：jsonwebtoken</span>
    </div>

    <img src="../assets/home.png">
    <span>注：被遮住的是 jwt.verify</span>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data() {
    return {
      src: '../assets/home.png'
    };
  },
  methods: {
    toLogin() {
      this.$router.push('login');
    }
  }
};
</script>

<style>
span {
  display: block;
  font-size: small;
}
</style>






