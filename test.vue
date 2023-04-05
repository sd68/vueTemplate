<template>

  <div style="padding: 30px">
    <div style="display: flex">
      <el-input size="large" style="width: 200px" v-model="searchKey" /><el-button @click="search()" size="large" type="primary" style="margin-left: 10px">搜索枣园</el-button>
      <el-button size="large" type="primary" @click="this.submitForm={};showAdd=true" style="margin-left: 10px">添加枣园</el-button>
    </div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column  label="id" width="100">
        <template #default="scope">
          <span >{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column label="名称" width="180">
        <template #default="scope">
          <span >{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column label="位置" width="300">
        <template #default="scope">
          <span >{{ scope.row.location }}</span>
        </template>
      </el-table-column>
      <el-table-column label="所有者" width="120">
        <template #default="scope">
          <span >{{ scope.row.owner }}</span>
        </template>
      </el-table-column>
      <el-table-column label="联系电话" width="120">
        <template #default="scope">
          <span >{{ scope.row.phone }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template #default="scope">
          <el-button  type="primary" @click="handleEdit(scope.$index, scope.row)"
                      size="large"
          >修改</el-button
          >
          <el-button
              type="danger"
              @click="DataDelete({id:scope.row.id},scope.row.name,'/farm/delete')"
              size="large"
          >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 20px;display: flex;justify-content: end;padding-right: 150px">
      <el-pagination :current-page="pageInfo.currentPage" :page-size="pageInfo.pageSize" :total="pageInfo.total"
                     @current-change="handleCurrentChange" background layout="prev, pager, next"/>
    </div>
  </div>
  <el-dialog style="width: 500px" v-model="showAdd" title="添加枣园">
    <el-form >
      <el-form-item label="枣园" >
        <el-input v-model="submitForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="位置" >
        <el-input v-model="submitForm.description" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="showAdd = false">取消</el-button>
        <el-button type="primary" @click="showAdd = false">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
  <el-dialog style="width: 500px" v-model="showEdit" title="修改枣园">
    <el-form >
      <el-form-item label="枣园" >
        <el-input v-model="submitForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="位置" >
        <el-input v-model="submitForm.description" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="showEdit = false">取消</el-button>
        <el-button type="primary" @click="showEdit = false">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script>
import { Timer } from '@element-plus/icons-vue'
import {getRequest} from "../../conf/myRequest";
import {ElMessage} from "element-plus";
import {DataDelete} from "../../conf/MyDelete";
export default {
  name: "Variety",
  components: {
    Timer,
  },
  data() {
    return {
      tableData: [],
      searchKey:'',
      pageInfo: {
        currentPage: 1
      },
      submitForm:{
        name:'',
        description:'',
      },
      showAdd:false,
      showEdit:false
    }
  },
  methods: {
    DataDelete,
    getList(){
      const that = this
      getRequest("/farm/list",{pageNum:that.pageInfo.currentPage,name:this.searchKey}).then(resp=>{
        that.tableData=resp.data.data.list
        that.pageInfo=resp.data.data
      })
    },
    search(){
      if(this.searchKey==''){
        ElMessage.warning('关键字不能为空')
      }else {
        this.getList()
      }
    },
    handleEdit(index, row) {
      this.showEdit=true
      this.submitForm={}
      this.submitForm=row
      console.log(index, row)
    },
    handleCurrentChange: function (currentPage) {
      console.log(currentPage)
      this.pageInfo.currentPage=currentPage
      this.getList()

    },
  },
  created() {
    this.getList()
  }
}
</script>

<style scoped>

</style>
