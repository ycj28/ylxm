<template>
   <div class="relativeList">
      <el-form :inline="true" :model="formInline" class="demo-form-inline" size="small">
         <el-form-item i>
            <el-input v-model="formInline.name" placeholder="输入亲属姓名或老人姓名查询"></el-input>
         </el-form-item>
         <el-form-item>
            <el-button type="primary" icon="el-icon-search" @click="find">查询</el-button>
         </el-form-item>
         <el-form-item>
            <el-button type="primary" icon="el-icon-refresh-right" @click="reset">重置</el-button>
         </el-form-item>
         <el-form-item>
            <el-button type="primary" icon="el-icon-circle-plus" @click="addRelative()">新增</el-button>
         </el-form-item>
      </el-form>

      <el-table :data="tableData" border style="width: 100%;">
         <el-table-column prop="name" label="姓名" align="center">
         </el-table-column>
         <el-table-column prop="ename" label="老人姓名" align="center">
         </el-table-column>
         <el-table-column prop="gender" label="性别" align="center">
         </el-table-column>
         <el-table-column prop="phone" label="电话号码" align="center">
         </el-table-column>
         <el-table-column prop="relationship" label="关系" align="center">
         </el-table-column>
         <el-table-column prop="address" label="家庭住址" align="center">
         </el-table-column>
         <el-table-column label="操作" align="center">
            <template slot-scope="scope">
               <el-button type="danger" size="mini" icon="el-icon-edit" @click="edit(scope.row)"></el-button>
               <el-button type="danger" size="mini" icon="el-icon-delete" @click="del(scope.row)"></el-button>
            </template>
         </el-table-column>
      </el-table>
      <Page :total="total" :url="url"></Page>


      <el-dialog :title="status ? '添加亲属信息' : '修改亲属信息'" :visible.sync="dialogFormVisible" width="500px">
         <el-form :model="form" :rules="rules" ref="form">
            <el-form-item label="姓名" :label-width="formLabelWidth" prop="name">
               <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="老人姓名" :label-width="formLabelWidth" prop="name">
               <el-input v-model="form.ename" autocomplete="off" :disabled="this.status ? false : true"></el-input>
            </el-form-item>
            <el-form-item label="性别" :label-width="formLabelWidth" prop="sex">
               <el-radio v-model="form.gender" label="男">男</el-radio>
               <el-radio v-model="form.gender" label="女">女</el-radio>
            </el-form-item>
            <el-form-item label="电话号码" :label-width="formLabelWidth" prop="age">
               <el-input v-model="form.phone" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="关系" :label-width="formLabelWidth" prop="father">
               <el-input v-model="form.relationship" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="家庭住址" :label-width="formLabelWidth" prop="address">
               <el-input v-model="form.address" autocomplete="off"></el-input>
            </el-form-item>
         </el-form>
         <div slot="footer" class="dialog-footer">
            <el-button @click="closeInfo('form')">取 消</el-button>
            <el-button type="primary" @click="sure('form')">确 定</el-button>
         </div>
      </el-dialog>

   </div>
</template>

<script>
import { delData, getData, changeInfo } from "../../utils/table.js"
import Page from '../common/Pageing.vue'
export default {
   components: {
      Page
   },
   data () {
      return {
         tableData: [],
         dialogFormVisible: false,
         total: 0,//
         url: '/relatives',
         formInline: {
            name: ''
         },
         form: {
            name: '',
            e_name: '',
            gender: '1',
            phone: '',
            relationship: '',
            address: '',
         },
         rules: {
            name: [{ required: true, message: '请输入姓名' }],
            gender: [{ required: true }],
            address: [{ required: true, message: '请输入地址' }],
            phone: [{ required: true, message: '请输入联系方式' }]
         },
         formLabelWidth: "80px",
         status: true,
      }
   },
   created () {
      getData(this, this.url)
   },
   methods: {
      find () {
         console.log(this.formInline)
         getData(this, '/relatives/byName', this.formInline)
      },
      reset () {
         this.formInline = {}
         getData(this, this.url, { page: 1, size: 10 })
      },
      edit (row) {
         this.status = false
         this, this.dialogFormVisible = true
         this.form = { ...row }
      },
      del (row) {
         console.log(row)
         delData(this, 'relatives', row.id, getData)
      },
      addRelative () {
         this.form = {
         },
            this.status = true
         this.dialogFormVisible = true
      },
      closeInfo (form) {
         this.dialogFormVisible = false
         this.$refs[form].resetFields()
      },
      sure (form) {
         this.$refs[form].validate(valid => {
            if (valid) {
               let methods = ''
               this.status ? methods = 'post' : methods = 'put'
               changeInfo(this, methods, this.url, this.form, getData)
            }
         })
      }
   }
}
</script>

<style lang="scss" scop>
.relativeList {

   .demo-form-inline,
   .el-form-item {
      margin-left: 20px;
      text-align: left;


   }

   .el-pagination {
      margin-top: 20px;
   }
}
</style>