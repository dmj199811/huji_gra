<template>
  <div>
    <el-table :data="tableData"  border>
      <el-table-column prop="id" label="Id"></el-table-column>
      <el-table-column prop="name" label="问卷名"></el-table-column>
      <el-table-column prop="userId" label="创建人id"></el-table-column>
      <el-table-column prop="username" label="创建人"></el-table-column>
      <el-table-column prop="createTime" label="创建时间"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="jumpInformation(scope.row.id)">查看数据</el-button>
          <el-button type="text" @click="jumpQuestionNaire(scope.row.id)">查看问卷</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
    layout="prev,pager,next,jumper"
    :total="data.length"
    :current-page.sync="pagination.page"
    @current-change='changePage'
    ></el-pagination>
  </div>
</template>
<script>
import { get } from "../../utils/utils.js";

export default {
  data(){
    return {
      tableData:[],
      data:[],
      pagination:{
        page:1
      }
    }
  },
  mounted(){
    this.handlerGetTableData()
  },
  methods:{
    jumpInformation(id){
      this.$router.push({path:'/point_information',query:{id}})
    },
    jumpQuestionNaire(id){
      window.open('https://www.baidu.com/','_blank')
    },
    changePage(){
      this.tableData = this.data.slice((this.pagination.page-1)*10,(this.pagination.page*10))
    },
    handlerGetTableData(){
      let id = localStorage.getItem('id')
      get('/api/question/user/'+id+'/survey').then((res)=>{
        if(res.data.code==200){
        this.data = res.data.data
        this.changePage()
        }
      }).catch(err=>{
        this.$message.error(err.message)
      })
    }
  }
}
</script>