<template>
  <div class="register-box">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="用户名" prop="name">
        <el-input v-model="form.name"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" show-password></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="checkPassword">
        <el-input v-model="form.checkPassword" show-password></el-input>
      </el-form-item>
      <el-form-item>
        <el-button class="register-btn" type="primary" @click="register" round>注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from '../axios.js';
export default {
  name: 'register',
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
    var validateCheckPass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'));
      } else if (value !== this.form.password) {
        callback(new Error('两次输入密码不一致!'));
      } else {
        callback();
      }
    };
    return {
      form: {
        name: '',
        password: '',
        checkPassword: ''
      },
      activeName: 'login',
      rules: {
        name: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 2, max: 10, message: '2~10 个字符', trigger: 'change' }
        ],
        password: [{ required: true, validator: validatePass, trigger: 'blur' }],
        checkPassword: [{ required: true, validator: validateCheckPass, trigger: 'blur' }]
      }
    };
  },
  methods: {
    register() {
      this.$refs['form'].validate(valid => {
        if (valid) {
          axios
            .userRegister(this.form)
            .then(({ data }) => {
              if (data.success) {
                this.$message({
                  type: 'success',
                  message: '注册成功'
                });
                this.$router.push('login');
              } else {
                this.$message({
                  type: 'info',
                  message: '用户名已经存在'
                });
              }
            })
            .catch(function(reason) {
              console.log('Failed: ' + reason);
            });
        }
      });
    }
  }
};
</script>

<style>
.register-box {
  display: flex;
  padding-top: 10%;
  margin-left: -5%;
  justify-content: center;
  align-items: center;
}

.register-btn {
  width: 45%;
  margin-left: -15%;
}
</style>


