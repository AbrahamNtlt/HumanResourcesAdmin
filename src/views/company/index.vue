<template>
  <div class="app-container">
    <el-button type="primary" size="small" class="app-button" @click="createCompany">新增公司</el-button>
    <el-table :data="list" v-loading.body="listLoading" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column label="公司名称">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="公司税号">
        <template slot-scope="scope">
          {{scope.row.tax_code}}
        </template>
      </el-table-column>
      <el-table-column
        align="center"
        label="操作"
        width="100">
        <template slot-scope="scope" >
          <el-button type="text" size="small" @click="updateCompany(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog :title="dialogTitle" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="公司名称" label-width="80px">
          <el-input v-model="form.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="公司税号" label-width="80px">
          <el-input v-model="form.tax_code" auto-complete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: null,
      listLoading: true,
      form: {
        name: '',
        tax_code: ''
      },
      dialogTitle: null,
      dialogFormVisible: false
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.list = [{
        id: 1,
        name: '北京精准沟通传媒科技股份有限公司',
        tax_code: '91112131337662767K'
      }, {
        id: 2,
        name: '深圳腾讯计算机系统有限公司',
        tax_code: '12321312397662767F'
      }]

      this.listLoading = false
    },
    createCompany() {
      this.dialogTitle = '新增公司'
      this.form = {
        name: '',
        tax_code: ''
      }
      this.dialogFormVisible = true
    },
    updateCompany(row) {
      this.dialogTitle = '修改公司'
      this.form = row
      this.dialogFormVisible = true
    }
  }
}
</script>
