<template>
  <div class="app-container">
    <el-button type="primary" size="small" class="app-button" @click="createCompany">新增报销</el-button>
    <el-table :data="list" v-loading.body="listLoading" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column label="报销名称">
        <template slot-scope="scope">
          {{scope.row.record_name}}
        </template>
      </el-table-column>
      <el-table-column label="报销分类">
        <template slot-scope="scope">
          {{scope.row.record_type}}
        </template>
      </el-table-column>
      <el-table-column label="报销公司">
        <template slot-scope="scope">
          {{scope.row.record_company}}
        </template>
      </el-table-column>
      <el-table-column label="报销金额">
        <template slot-scope="scope">
          {{scope.row.record_money}}
        </template>
      </el-table-column>
      <el-table-column label="报销状态">
        <template slot-scope="scope">
          <el-tag v-if="scope.row.record_status == 101" size="small" type="danger">未开票</el-tag>
          <el-tag v-else-if="scope.row.record_status == 102" size="small" type="warning">未提交</el-tag>
          <el-tag v-else-if="scope.row.record_status == 103" size="small" type="warning">已提交</el-tag>
          <el-tag v-else-if="scope.row.record_status == 104" size="small" type="warning">已通过</el-tag>
          <el-tag v-else-if="scope.row.record_status == 105" size="small" type="warning">未到账</el-tag>
          <el-tag v-else-if="scope.row.record_status == 106" size="small" type="success">已到账</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="购买时间">
        <template slot-scope="scope">
          {{scope.row.record_buy}}
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
        <el-form-item label="报销名称" label-width="80px">
          <el-input v-model="form.record_name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="报销分类" label-width="80px">
          <el-input v-model="form.record_type" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="报销公司" label-width="80px">
          <el-select v-model="form.record_company" placeholder="请选择">
            <el-option
              v-for="item in optionsCompany"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="报销金额" label-width="80px">
          <el-input v-model="form.record_money" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="报销状态" label-width="80px">
          <el-select v-model="form.record_status" placeholder="请选择">
            <el-option
              v-for="item in optionsStatus"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="购买时间" label-width="80px">
          <el-date-picker
            v-model="form.record_buy"
            type="datetime"
            placeholder="选择日期时间">
          </el-date-picker>
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
      optionsCompany: [{
        value: '腾讯科技',
        label: '腾讯科技'
      }, {
        value: '精准科技',
        label: '精准科技'
      }, {
        value: '今日头条',
        label: '今日头条'
      }, {
        value: '泰岳梧桐',
        label: '泰岳梧桐'
      }],
      optionsStatus: [{
        value: 101,
        label: '未开票'
      }, {
        value: 102,
        label: '未提交'
      }, {
        value: 103,
        label: '已提交'
      }, {
        value: 104,
        label: '已通过'
      }, {
        value: 105,
        label: '未到账'
      }, {
        value: 106,
        label: '已到账'
      }]
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.list = [{
        record_id: 1,
        record_name: '苹果笔记本',
        record_type: '办公用品',
        record_company: '宁波泰岳',
        record_money: '13800',
        record_status: 103,
        record_buy: '2018-03-01 12:00'
      }]

      this.listLoading = false
    },
    createCompany() {
      this.dialogTitle = '新增报销'
      this.form = {
        record_id: '',
        record_name: '',
        record_type: '',
        record_company: '',
        record_money: '',
        record_status: '',
        record_buy: ''
      }
      this.dialogFormVisible = true
    },
    updateCompany(row) {
      this.dialogTitle = '修改报销'
      this.form = row
      this.dialogFormVisible = true
    }
  }
}
</script>
