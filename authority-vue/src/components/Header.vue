<template>
  <div style="line-height: 60px;display: flex">
    <div style="flex: 1;font-size: 30px">
      <span :class="collapseBtnClass" style="cursor: pointer" @click="collapse"></span>
      <el-breadcrumb separator="/" style="display: inline-block;margin-left:10px;position: absolute;top:22px" >
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item><b>{{currentPathName}}</b></el-breadcrumb-item>
      </el-breadcrumb>
    </div>


    <el-dropdown style="width: 150px;cursor: pointer;text-align: center">
      <div style="display: inline-block">
        <img :src="user.headerUrl" style="width:65px;height:45px;border-radius: 50%;position: relative;top:10px;right:8px"/>
        <span><b>{{user.username}}</b></span><i class="el-icon-arrow-down" style="margin-left:5px"></i>
      </div>

      <el-dropdown-menu slot="dropdown" style="width: 100px;text-align: center;text-decoration: none">
        <el-dropdown-item style="font-size: 15px;padding:5px 0">
          <router-link to="/person" style="text-decoration: none;color: #606266">个人信息</router-link>
        </el-dropdown-item>
        <el-dropdown-item style="font-size: 15px;padding:5px 0">
          <router-link to="/password" style="text-decoration: none;color: #606266">修改密码</router-link>
        </el-dropdown-item>
        <el-dropdown-item style="font-size: 15px;padding:5px 0">
          <div style="text-decoration: none" @click="logout">退出登录</div>
        </el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

  </div>
</template>

<script>
import router from "@/router";

export default {
  name: "Header",
  props:{
    collapseBtnClass:String,
    user:Object
  },
  computed:{
    currentPathName(){
      return this.$store.state.currentPathName;
    }
  },
  methods:{
    collapse(){
      this.$emit('collapse')
    },
    logout(){
      this.request.post('/user/logout').then(res => {
        if(res.code === '200'){
          this.$store.commit('logout');
          this.$message.success('退出登录成功');
        }else{
          this.$message.error('退出登录失败');
        }
      })
    }
  }
}
</script>

<style scoped>

</style>