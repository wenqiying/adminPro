<template>
  <div style="padding: 10px">
    <div style="margin: 10px 0px">
      <el-button type="primary" @click="add">新增</el-button>
      <el-button type="primary">导入</el-button>
      <el-button type="primary">导出</el-button>
    </div>
<!--    搜索区域-->
    <div style="margin: 10px 0px">
      <el-input v-model="search" placeholder="请输入关键字" clearable style="width: 20%"/>
      <el-button type="primary" style="margin-left: 5px" @click="load">查询</el-button>
    </div>
    <el-table
            :data="tableData"
            style="width: 100%"
            stripe
            :row-class-name="tableRowClassName"
    >
      <el-table-column prop="id" label="ID" sortable />
      <el-table-column prop="username" label="Name"  />
      <el-table-column prop="nickName" label="NickName"  />
      <el-table-column prop="age" label="Age" />
      <el-table-column prop="sex" label="Sex" />
      <el-table-column prop="address" label="Address" />
      <el-table-column fixed="right" label="Operations" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
          <el-popconfirm title="Are you sure to delete this?" @confirm="handleDelete(scope.row.id)">
            <template #reference>
              <el-button link type="primary" size="small">删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div>
      <el-pagination
            v-model:currentPage="currentPage"
            v-model:page-size="pageSize"
            :page-sizes="[5, 10, 20]"
            :small="small"
            :disabled="disabled"
            :background="background"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
      />
      <el-dialog v-model="dialogVisible" title="Tips" width="30%">
        <el-form :model="form" label-width="120px">
          <el-form-item label="UserName">
            <el-input v-model="form.username" style="width: 80%"/>
          </el-form-item>
          <el-form-item label="Password">
            <el-input v-model="form.password" style="width: 80%"/>
          </el-form-item>
          <el-form-item label="NickName">
            <el-input v-model="form.nickName" style="width: 80%"/>
          </el-form-item>
          <el-form-item label="Age">
            <el-input v-model="form.age" style="width: 80%"/>
          </el-form-item>
          <el-form-item label="Sex">
            <el-radio-group v-model="form.sex" style="width: 80%">
              <el-radio label="男" />
              <el-radio label="女" />
              <el-radio label="未知" />
            </el-radio-group>
          </el-form-item>
          <el-form-item label="Address">
            <el-input type="textarea" v-model="form.address" style="width: 80%"/>
          </el-form-item>
        </el-form>
        <template #footer>
            <span class="dialog-footer">
              <el-button @click="dialogVisible = false">Cancel</el-button>
              <el-button type="primary" @click="save">Confirm</el-button>
            </span>
        </template>
      </el-dialog>
    </div>
  </div>
</template>

<script>

import request from "../utils/request";

export default {
  name: 'Home',
  components: {
  },
  data(){
    return{
      input:"",
      form:{},
      dialogVisible:false,
      currentPage:1,
      pageSize:10,
      total:0,
      search: '',
      tableData:[]
    }
  },
  created() {
    this.load()
  },
  methods:{
    handleDelete(id){
      request.delete("/user/"+id).then(res => {
        if(res.code == '0'){
          this.$message({
            type:"success",
            message:"更新成功"
          })
        }else {
          this.$message({
            type:"success",
            message:res.msg
          })
        }
        this.load()
      })
    },
    handleSizeChange(){
      this.load()
    },
    handleCurrentChange(){
      this.load()
    },
    handleEdit(row){
      this.form = JSON.parse(JSON.stringify(row))
      this.dialogVisible = true
    },
    load(){
      request.get("/user",{
        params:{
          pageNum:this.currentPage,
          pageSize:this.pageSize,
          search:this.search
        }
      }).then(res => {
        console.log(res)
        this.tableData=res.data.records
        this.total=res.data.total
      })
    },
    add(){
      this.dialogVisible=true
      this.form={}
    },
    save(){
      if(this.form.id){

        request.put("/user",this.form).then(res => {
          console.log(res)
          if(res.code == '0'){
            this.$message({
              type:"success",
              message:"更新成功"
            })
          }else {
            this.$message({
              type:"success",
              message:res.msg
            })
          }
          this.load()
          this.dialogVisible = false

        })
      }else {
        request.post("/user",this.form).then(res => {
          console.log(res)
          if(res.code == '0'){
            this.$message({
              type:"success",
              message:"新增成功"
            })
          }else {
            this.$message({
              type:"success",
              message:res.msg
            })
          }
        })
        this.load()
        this.dialogVisible = false
      }
    }
  }

}
</script>
