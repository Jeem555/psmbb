<template>
  <div>
    <div style="margin-left: 300px;margin-top:30px;margin-bottom: 30px;">
      <!--      搜索栏-->
      搜索栏:
      <el-input  style="width: 200px;margin-right: 20px" placeholder="请输入文章名称" v-model="name" prefix-icon="el-icon-user"></el-input>
      <el-input  style="width: 200px;margin-right: 20px" placeholder="请输入创建者名称" v-model="user" prefix-icon="el-icon-user"></el-input>
      <el-button style="margin-left: 10px;" type="primary" @click="load" class="el-icon-search">搜索</el-button>
      <el-button style="margin-left: 10px;" type="warning" @click="reset" class="el-icon-refresh">重置</el-button>
    </div>

<!--    文章列表-->
    <div style="width: 80%;margin: auto">
      <el-row :gutter="2">
        <el-col :span="4" v-for="(article,index) in articleList" :key="index">
          <div style="margin-left:10px; margin-bottom: 45px">
            <router-link target="_blank" :to="{path:'/front/articleDetail',query:{id:article.id}}">
              <el-image
                  style="width: 180px; height: 180px"
                  :src="article.url">
              </el-image>
<!--              <img :src="article.url" style="width: 100%;height: 100%;cursor: pointer;" title="查看文章">-->
            </router-link><br/><br/>
            <div class="el-icon-document">文章名称:<span style="color:red;margin-left:10px">{{article.name}}</span></div><br/><br/>
            <div class="el-icon-user">创建者名称:<span style="color:red;margin-left:10px">{{article.user}}</span></div><br/><br/>
            <div class="el-icon-time">创建时间：<span style="color:red;">{{article.createTime.split(' ')[0]}}</span></div>
          </div>
        </el-col>
      </el-row>
    </div>

<!--    分页-->
    <div class="block" style="padding:10px 0;align-content: center;margin-left: 30%;margin-top:30px;">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-sizes="[6, 12, 18, 24]"
          :page-size="6"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  created() {
    this.load();
  },
  data(){
    return{
      articleList:[],
      pageNum:1,
      pageSize:6,
      total:0,
      name:'',
      user:''
    }
  },
  methods:{
    load(){
      this.request.get("/article/findPage",{
        params:{
          pageNum:this.pageNum,
          pageSize:this.pageSize,
          name:this.name,
          user:this.user
        }
      }).then(res => {
        this.articleList = res.data.records;
        this.total = res.data.total;
      })
    },
    reset(){
      this.user = '';
      this.name = '';
      this.load();
    },
    handleSizeChange(val) {
      this.pageSize = val;
      this.load();
    },
    handleCurrentChange(val) {
      this.pageNum = val;
      this.load();
    },
  }
}
</script>

<style scoped>

</style>