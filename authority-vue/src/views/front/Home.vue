<template>
  <div>
<!--    轮播图展示-->
    <div style="width:80%;margin:auto">
      <el-carousel :interval="5000" arrow="always">
        <el-carousel-item v-for="(item,index) in imgList" :key="index">
          <el-image
              style="width: 100%; height: 100%"
              :src="item"
              :preview-src-list="[item]">
          </el-image>
        </el-carousel-item>
      </el-carousel>
    </div>

    <div style="margin-top: 50px;display: flex">
      <div style="height: 90%;width: 50%;margin-right: 2%;margin-left: 10%;justify-content: center">
        <el-card style="height: 240px;align-content: center;text-align: center">
          本项目是基于springboot + vue的前后端分离权限项目，手把手搭建。<br/>

          后端技术栈：springboot，MySQL，redis，Maven<br/>

          前端技术栈：vue，Vuex,Vue-Router,Echarts,elementUI<br/>

          通用的权限管理项目，可以在本系统的基础上开发大部分的管理系统。<br/>

          需要项目资料可以私聊我，免费提供，可以无偿辅导java和前端到就业水平<br/>
          <a href="https://space.bilibili.com/56136258?spm_id_from=333.999.0.0">视频地址</a>
        </el-card>
      </div>

      <div style="width: 20%;display: flex;padding-right: 30px">
        <el-card style="width: 100%;background-color: #eeeeee;height: 240px;">
            最新公告 <span style="color: #42b983;cursor: pointer;margin-left: 80px" @click="$router.push('/front/notice')">查看更多...</span>
          <div v-for="(notice,index) in newNoticeList" :key="index" class="text item" style="margin: 20px">
            {{notice.title}} <span style="color: #42b983;cursor: pointer;margin-left: 80px" @click="viewNotice(notice.id)">查看公告</span>
          </div>
        </el-card>
      </div>

      <el-dialog title="公告内容" :visible.sync="dialogFormVisible" width="30%">
       <el-card>
          {{content}}
       </el-card>
      </el-dialog>


    </div>

  </div>
</template>

<script>
export default {
  name: "Home",
  created() {
    this.request.get('/lunbo/getAllUrlList').then(res => {
      if(res.code === '200'){
        this.imgList = res.data;
      }else{
        this.$message.error('获取轮播图数据失败，请联系管理员')
      }
    })
    this.request.get('/notice/findNewNoticeList').then(res => {
      if(res.code === '200'){
        this.newNoticeList = res.data;
      }else{
        this.$message.error('获取公告数据失败，请联系管理员')
      }
    })
  },
  data(){
    return{
      imgList:[],
      newNoticeList:[],
      dialogFormVisible:false,
      content:''
    }
  },
  methods:{
    viewNotice(id){
      this.request.get('/notice/findById/' + id).then(res => {
        if(res.code === '200'){
          this.content = res.data.content
        }else{
          this.$message.error('获取公告内容失败，请联系管理员')
        }
      })
      this.dialogFormVisible = true;

    }
  }
}
</script>

<style scoped>
.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

.box-card {
  width: 480px;
}
</style>