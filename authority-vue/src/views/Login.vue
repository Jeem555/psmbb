<template>
  <div class="wrapper">

<!--    项目名称-->
    <div style="height: 60px;line-height: 60px;font-size: 20px;padding-left: 50px;color: white;text-align: center;align-content: center;background-color: #cccccc">
      权限脚手架项目
    </div>

    <div style="display: flex;width:55%;height:50%;margin: 150px auto;background-color: white;border-radius: 10px;overflow: hidden;background-color: bisque">
<!--      左侧图片显示-->
      <div style="width: 50%;margin-top: 30px;margin-left: 30px">
        <img src="../assets/register.png" alt="" style="width:100%;height:90%" />
      </div>
<!--      提交表单-->
      <div style="width:350px;margin-top: 30px">
        <div style="margin-top: 20px;text-align: center;font-size: 20px;margin-bottom: 10%">登录页面</div>
        <el-form :model="userForm" :rules="rules" ref="userForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="用户名" prop="username">
            <el-input v-model="userForm.username" prefix-icon="el-icon-user"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input v-model="userForm.password" prefix-icon="el-icon-lock" type="password" show-password></el-input>
          </el-form-item>
          <el-form-item label="验证码" prop="identifyCode">
            <el-input v-model="userForm.identifyCode" style="width:50%;position:relative;bottom:8px;" prefix-icon="el-icon-mobile-phone"></el-input>
            <span @click="refreshCode" style="position: relative;top:5px;left:6px;">
              <SIdentity :identify-code="identifyCode"></SIdentity>
            </span>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="login('userForm')" style="margin-left:20px">登录</el-button>
            <el-button type="success" @click="$router.push('/register')">前往注册</el-button>
<!--            <el-button type="warning" @click="resetForm('userForm')">重置</el-button>-->
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import SIdentity from "@/components/SIdentity";
import {setRoutes} from "@/router";
export default {
  name: "Login",
  components:{
    SIdentity
  },
  mounted() {
    this.refreshCode()
  },
  data(){
    return{
      userForm:{},
      identifyCode:'',
      identifyCodes:'1234567890zxcvbnmasdfghjklqwertyuiopQWERTYUIPOLSADFGHJ',
      rules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 50, message: '长度不能小于6位', trigger: 'blur' }
        ],
        identifyCode: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { min: 4, max: 4, message: '验证码长度为4位', trigger: 'blur' }
        ]
      }
    }
  },
  methods:{
    //提交数据
    login(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if(this.identifyCode.toUpperCase() !== this.userForm.identifyCode.toUpperCase()){
            this.$message.warning("验证码输入错误，请重新输入");
            return;
          }
          this.request.post("/user/login",this.userForm).then(res => {
              if(res.code !== '200'){
                this.$message.error(res.msg);
              }else{
                localStorage.setItem("user",JSON.stringify(res.data))
                //保存菜单信息
                localStorage.setItem("menus",JSON.stringify(res.data.menuList))
                this.$message.success("登录成功");
                setRoutes();
                this.$router.push("/")
              }
          })
        } else {
          console.log('输入数据不正确，请重新输入');
          return false;
        }
      });
    },
    // 重置数据
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },

    // 验证码方法
    refreshCode(){
      this.identifyCode = ''
      this.makeCode(this.identifyCodes,4);
    },
    makeCode(o,l){
      for(let i = 0;i < l;i++){
        this.identifyCode += this.identifyCodes[
              this.randomNum(0,this.identifyCodes.length)
            ]
      }
    },
    randomNum(min,max){
      return Math.floor(Math.random() * (max - min) + min);
    }
  }
}
</script>

<style scoped>
  .wrapper{
    background: url("../assets/auth.jpg");
    width:100%;
    height: 100%;
    position: fixed;
    background-size: 100% 100%;
  }
</style>