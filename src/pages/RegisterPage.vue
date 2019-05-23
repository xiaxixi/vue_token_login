<template>
  <div id="login-register-box">
    <el-tabs v-model="activeName" @tab-click="handleClick" stretch>
      <el-tab-pane label="登陆" name="login">
        <login></login>
      </el-tab-pane>
      <el-tab-pane label="注册" name="register">
        <register></register>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import login from '@/components/login';
import register from '@/components/register';
export default {
  name: 'LoginRegister',
  data() {
    return {
      activeName: 'register'
    };
  },
  components: { login, register },
  methods: {
    handleClick(tab, event) {
      this.$router.push('login');
    }
  }
};
</script>

<style>
#login-register-box {
  width: 400px;
  height: auto;
  margin: auto;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
}

.el-tabs__nav-wrap .el-tabs__nav-scroll .el-tabs__nav {
  width: 100%;
  display: flex;
  justify-content: space-around;
}
</style>
