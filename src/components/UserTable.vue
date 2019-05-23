<template>
  <el-table class="user-table" :data="users" border>
    <el-table-column prop="username" label="username" width="120px"></el-table-column>
    <el-table-column prop="create_time" label="create_time" width="200px"></el-table-column>
    <el-table-column label="操作" width="100px">
      <template slot-scope="scope">
        <el-button type="text" size="small" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import axios from '../axios.js';
export default {
  name: 'UserTable',
  data() {
    return {
      users: []
    };
  },
  methods: {
    handleDelete(index, row) {
      let thisID = {
        id: this.users[index]._id
      };
      axios
        .delUser(thisID)
        .then(response => {
          this.$message({
            type: 'success',
            message: '删除成功'
          });
          //移除节点
          this.users.splice(index, 1);
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  created() {
    axios
      .getUser()
      .then(response => {
        if (response.status === 401) {
          //不成功跳转回登录页
          this.$router.push('/login');
          //并且清除掉这个token
          this.$store.disptch('UserLogout');
        } else if (response.status === 404) {
          console.log(404);
        } else {
          //成功了就把data.result里的数据放入users，在页面展示
          this.users = response.data.result;
          console.log(this.users);
        }
      })
      .catch(err => {
        console.log(err);
      });
  }
};
</script>

<style>
.user-table {
  display: block;
  width: 421px;
  margin: auto;
  margin-bottom: 20px;
}
</style>
