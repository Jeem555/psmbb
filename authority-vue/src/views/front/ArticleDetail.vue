<template>
  <div style="color: #666">
    <div style="text-align: center;margin: 40px;font-size: 30px">
      <h1 style="margin: 20px;text-align: center">文章详情</h1>
      <span class="el-icon-user">创建者名称:{{article.user}}</span><br/>
      <span class="el-icon-time">创建时间:{{article.createTime}}</span>
    </div>

    <el-card style="width: 88%;margin: auto">
      <mavon-editor
          class="md"
          :value="article.content"
          :subfield="false"
          :defaultOpen="'preview'"
          :toolbarsFlag="false"
          :editable="false"
          :scrollStyle="true"
          :ishljs="true"
      />
    </el-card>

  </div>
</template>

<script>
export default {
  name: "ArticleDetail",
  created() {
    console.log(typeof this.id)
    this.request.get('/article/findById/' + this.id).then(res => {
      if(res.code === '200'){
        this.article = res.data;
      }else{
        this.$message.error('找不到文章信息');
      }
    })
  },
  data(){
    return{
      id:this.$route.query.id,
      article:{}
    }
  },
}
</script>

<style scoped>

</style>