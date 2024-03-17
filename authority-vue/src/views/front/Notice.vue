<template>
  <div style="width: 88%;margin:auto">
    <div style="margin: 30px">
      <h1 style="text-align: center">公告列表</h1>
      <!--      搜索栏-->
      <el-input  style="width: 200px;margin-right: 20px" placeholder="请输入标题" v-model="title" prefix-icon="el-icon-user"></el-input>
      <el-input  style="width: 200px;margin-right: 20px" placeholder="请输入创建者" v-model="user" prefix-icon="el-icon-user"></el-input>
      <el-button style="margin-left: 10px;" type="primary" @click="load" class="el-icon-search">搜索</el-button>
      <el-button style="margin-left: 10px;" type="warning" @click="reset" class="el-icon-refresh">重置</el-button>
    </div>

    <el-table :data="tableData" border stripe :header-cell-style="getRowClass" @selection-change="handleSelectionChange">
      <el-table-column prop="title" label="公告标题"></el-table-column>
      <el-table-column prop="content" label="公告内容">
        <template slot-scope="scope">
          <el-card>{{scope.row.content}}</el-card>
        </template>
      </el-table-column>
      <el-table-column prop="user" label="创建者"></el-table-column>
      <el-table-column prop="createTime" label="创建时间"></el-table-column>
    </el-table>

    <div class="block" style="padding:10px 0;align-content: center;margin-left: 30%;margin-top:30px;">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-sizes="[5, 10, 15, 20]"
          :page-size="10"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
      </el-pagination>
    </div>


  </div>
</template>

<script>
export default {
  name: "Notice",
  data(){
    return{
      pageSize:10,
      pageNum:1,
      title:'',
      user:'',
      tableData:[],
      dialogFormVisible:false,
      form:{},
      formLabelWidth: '80px',
      multipleSelection:[],
      total:0
    }
  },
  created() {
    this.load();
  },
  methods:{
    load(){
      this.request.get("/notice/findPage",{
        params:{
          pageNum:this.pageNum,
          pageSize:this.pageSize,
          title:this.title,
          user:this.user,
        }
      }).then(res => {
        this.tableData = res.data.records;
        this.total = res.data.total;
      })
    },
    getRowClass({rowIndex,columnIndex}){
      if(rowIndex === 0){
        return 'background:#ccc'
      }
    },
    reset(){
      this.title = '';
      this.user = '';
      this.load();
    },
    save(){
      this.dialogFormVisible = true;
      this.form = {};
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
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