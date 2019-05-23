<template>
  <div class="login-box">
    <el-form ref="form" :model="form" :rules="rules" label-width="70px">
      <el-form-item label="用户名" prop="name">
        <el-input v-model="form.name" clearable></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" show-password></el-input>
      </el-form-item>
      <el-form-item>
        <el-button class="login-btn" type="primary" @click="login" round>登陆</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from '../axios.js';
export default {
  name: 'login',
  data() {
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else {
        if (this.form.checkpassword !== '') {
          this.$refs.form.validateField('checkPassword');
        }
        callback();
      }
    };
    return {
      form: {
        name: '',
        password: ''
      },
      rules: {
        name: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 2, max: 10, message: '2~10 个字符', trigger: 'change' }
        ],
        password: [{ required: true, validator: validatePass, trigger: 'blur' }]
      }
    };
  },
  methods: {
    login() {
      axios
        .userLogin(this.form)
        .then(({ data }) => {
          //账号不存在
          if (data.info === false) {
            this.$message({
              type: 'info',
              message: '账号不存在'
            });
            this.$router.push('register');
            return;
          }
          //账号存在
          if (data.success) {
            this.$message({
              type: 'success',
              message: '登陆成功'
            });
            //拿到返回的token和username，并存到store
            let token = data.token;
            let username = data.username;
            this.$store.dispatch('UserLogin', token);
            this.$store.dispatch('UserName', username);
            //跳到目标页
            this.$router.push('page1');
          }
        })
        .catch(function(reason) {
          console.log('Failed: ' + reason);
        });
    }
  }
};
</script>

<style>
.login-box {
  display: flex;
  padding-top: 10%;
  justify-content: center;
  align-items: center;
}

.login-btn {
  width: 45%;
  margin-left: -10%;
}
</style>


