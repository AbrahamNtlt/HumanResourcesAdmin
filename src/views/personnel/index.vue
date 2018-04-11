<template>
  <div class="app-container">
    <el-button type="primary" size="small" class="app-button" @click="createCompany">新增员工</el-button>
    <el-table :data="list" v-loading.body="listLoading" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column label="姓名">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="身份证号">
        <template slot-scope="scope">
          {{scope.row.code}}
        </template>
      </el-table-column>
      <el-table-column label="签约公司">
        <template slot-scope="scope">
          {{scope.row.company}}
        </template>
      </el-table-column>
      <el-table-column label="电话号码">
        <template slot-scope="scope">
          {{scope.row.phone}}
        </template>
      </el-table-column>
      <el-table-column label="入职时间">
        <template slot-scope="scope">
          {{scope.row.entry_time}}
        </template>
      </el-table-column>
      <el-table-column label="状态">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.status == 1" size="small">在职</el-tag>
          <el-tag v-else type="info" size="small">离职</el-tag>
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
        <el-form-item label="姓名" label-width="80px">
          <el-input v-model="form.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="身份证号" label-width="80px">
          <el-input v-model="form.code" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="签约公司" label-width="80px">
          <el-input v-model="form.company" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话号" label-width="80px">
          <el-input v-model="form.phone" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="入职时间" label-width="80px">
          <el-date-picker
            v-model="form.entry_time"
            type="date"
            placeholder="选择日期">
          </el-date-picker>
        </el-form-item>
        <el-form-item label="在职状态" label-width="80px">
          <el-select v-model="form.status" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
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
      dialogFormVisible: false,
      options: [{
        value: 1,
        label: '在职'
      }, {
        value: 0,
        label: '离职'
      }]
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.list = [{
        id: 1,
        name: '张闯',
        code: '210111198806165310',
        company: '精准科技',
        phone: '18512223234',
        entry_time: '2016-06-06',
        status: 1
      }]

      this.listLoading = false
    },
    createCompany() {
      this.dialogTitle = '新增员工'
      this.form = {
        name: '',
        tax_code: ''
      }
      this.dialogFormVisible = true
    },
    updateCompany(row) {
      this.dialogTitle = '编辑员工'
      this.form = row
      this.dialogFormVisible = true
    }
  }
}
</script>
