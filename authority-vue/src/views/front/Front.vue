<template>
  <div>


    <div style="display: flex;height:100px;line-height: 60px;border: 1px solid #eee">
      <!--    左侧的logo和文字-->
      <div style="width:300px;display: flex;padding-left: 30px">
        <div style="width: 60px">
          <img src="../../assets/register.png" alt="前台logo" style="width:60px;position: relative;top:20px;"/>
        </div>
        <div style="width: 30vh;position: relative;top: 18px;margin-left: 10px;margin-right: 20px;">权限脚手架系统前台页面</div>
      </div>

<!--      导航栏-->
      <div style="flex: 1;margin-top: 10px;margin-bottom: 10px;">
        <el-card style="height: 80px;width: 130vh;text-align: center;display:flex;justify-content: center">
          <el-menu
              class="el-menu-demo"
              style="display: flex;justify-content: center;line-height: 30px;"
              mode="horizontal"
              active-text-color="#ffd04b"
              router
          >
            <el-menu-item index="/front/home" style="font-size: 26px">首页</el-menu-item>
            <el-menu-item index="/front/article" style="font-size: 26px">文章列表</el-menu-item>
            <el-menu-item index="/front/notice" style="font-size: 26px">公告列表</el-menu-item>
          </el-menu>
        </el-card>
      </div>

<!--      用户头像和用户名以及下拉框-->
      <div style="width:20vh;position: relative;top:16px;right:30px;">
        <div v-if="user">
          <el-dropdown style="width: 150px;cursor: pointer;text-align: center">
            <div style="display: inline-block">
              <img :src="user.headerUrl" style="width:65px;height:45px;border-radius: 50%;position: relative;top:10px;right:8px"/>
              <span><b>{{user.username}}</b></span><i class="el-icon-arrow-down" style="margin-left:5px"></i>
            </div>

            <el-dropdown-menu slot="dropdown" style="width: 100px;text-align: center;text-decoration: none">
              <el-dropdown-item style="font-size: 15px;padding:5px 0">
                <router-link to="/front/person" style="text-decoration: none;color: #606266">个人信息</router-link>
              </el-dropdown-item>
              <el-dropdown-item style="font-size: 15px;padding:5px 0">
                <router-link to="/front/password" style="text-decoration: none;color: #606266">修改密码</router-link>
              </el-dropdown-item>
              <el-dropdown-item style="font-size: 15px;padding:5px 0">
                <div style="text-decoration: none" @click="logout">退出登录</div>
              </el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
        <div v-else style="position: relative;right: 30px;left: 50px">
          <el-button type="primary" @click="$router.push('/login')">前往登录</el-button>
        </div>
      </div>

    </div>

    <div>
      <router-view @refreshUser="getUser"></router-view>
    </div>
  </div>
</template>

<script>
export default {
  name: "Front",
  data(){
    return{
      user:null
    }
  },
  created() {
    this.getUser();
  },
  methods:{
    logout(){
      this.request.post('/user/logout').then(res => {
        if(res.code === '200'){
          this.$store.commit('logout');
          this.$message.success('退出登录成功');
        }else{
          this.$message.error('退出登录失败');
        }
      })
    },
    getUser(){
      let id = localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')).id : null;
      if(id){
        this.request.get("/user/findById/" + id).then(res => {
          if(res.code === '200'){
            this.user = res.data;
          }else{
            this.$message.error('找不到用户信息');
          }
        })
      }
    }
  }

}
</script>

<style scoped>

</style>