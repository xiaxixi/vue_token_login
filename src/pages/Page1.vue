<template>
  <div>
    <div>
      <UserTable></UserTable>
    </div>
    <el-button type="primary" @click="logout()">注销</el-button>
  </div>
</template>

<script>
import axios from '../axios.js';
import UserTable from '@/components/UserTable';
export default {
  name: 'Page1',
  methods: {
    logout() {
      // 清除token
      this.$store.dispatch('UserLogout');
      if (!this.$store.state.token) {
        this.$router.push('/login');
        this.$message({
          type: 'success',
          message: '注销成功'
        });
      } else {
        this.$message({
          type: 'info',
          message: '注销失败'
        });
      }
    }
  },
  components: { UserTable }
};
</script>

<style>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>