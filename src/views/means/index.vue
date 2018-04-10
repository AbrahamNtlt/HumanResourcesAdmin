<template>
  <div class="app-container">
    <el-button type="primary" size="small" class="app-button" @click="createAssets">新增资产</el-button>
    <el-table :data="list" v-loading.body="listLoading" element-loading-text="Loading" border fit highlight-current-row>
      <el-table-column label="资产名称">
        <template slot-scope="scope">
          {{scope.row.name}}
        </template>
      </el-table-column>
      <el-table-column label="型号">
        <template slot-scope="scope">
          {{scope.row.model}}
        </template>
      </el-table-column>
      <el-table-column label="数量">
        <template slot-scope="scope">
          {{scope.row.amount}}
        </template>
      </el-table-column>
      <el-table-column label="所属公司">
        <template slot-scope="scope">
          {{scope.row.company}}
        </template>
      </el-table-column>
      <el-table-column label="所属部门">
        <template slot-scope="scope">
          {{scope.row.department}}
        </template>
      </el-table-column>
      <el-table-column label="购买时间">
        <template slot-scope="scope">
          {{scope.row.buy_time}}
        </template>
      </el-table-column>
      <el-table-column
        align="center"
        label="操作"
        width="150">
        <template slot-scope="scope" >
          <el-button type="text" size="small" @click="dialogReceiveVisible = true">领取</el-button>
          <el-button type="text" size="small" @click="updateAssets(scope.row)">编辑</el-button>
          <el-button type="text" size="small" @click="dialogShowVisible = true">查看</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog :title="dialogTitle" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="资产名称" label-width="80px">
          <el-input v-model="form.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="型号" label-width="80px">
          <el-input v-model="form.model" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="数量" label-width="80px">
          <el-input v-model="form.amount" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="所属公司" label-width="80px">
          <el-input v-model="form.company" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="所属部门" label-width="80px">
          <el-input v-model="form.department" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="购买时间" label-width="80px">
          <el-date-picker
            v-model="form.buy_time"
            type="date"
            placeholder="选择日期">
          </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog title="资产领取" :visible.sync="dialogReceiveVisible">
      <el-form :model="receive">
        <el-form-item label="资产名称" label-width="80px">
          <el-input v-model="receive.name" auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="领取人" label-width="80px">
          <el-select v-model="receive.personnel" placeholder="领取人">
            <el-option
              v-for="item in personnelList"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="所属公司" label-width="80px">
          <el-select v-model="receive.company" placeholder="所属公司">
            <el-option
              v-for="item in companylList"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="所属部门" label-width="80px">
          <el-select v-model="receive.department" placeholder="所属部门">
            <el-option
              v-for="item in departmentList"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="领取时间" label-width="80px">
          <el-date-picker
            v-model="receive.time"
            type="date"
            placeholder="选择日期">
          </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogReceiveVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogReceiveVisible = false">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog title="领取情况" :visible.sync="dialogShowVisible">
      <el-table :data="gridData">
        <el-table-column property="name" label="领取人"></el-table-column>
        <el-table-column property="time" label="领取时间"></el-table-column>
        <el-table-column property="amount" label="领取数量"></el-table-column>
        <el-table-column
          align="center"
          label="操作"
          width="100">
          <template slot-scope="scope" >
            <el-button type="text" size="small">归还</el-button>
          </template>
        </el-table-column>
      </el-table>
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
        model: '',
        amount: '',
        company: '',
        department: '',
        buy_time: ''
      },
      receive: {
        name: '',
        personnel: '',
        company: '',
        department: '',
        time: ''
      },
      dialogTitle: null,
      dialogFormVisible: false,
      dialogReceiveVisible: false,
      dialogShowVisible: false,
      personnelList: [{
        value: 101,
        label: '丁艳秋'
      }, {
        value: 102,
        label: '姜婷'
      }],
      companylList: [{
        value: 1010,
        label: '精准科技'
      }, {
        value: 1020,
        label: '神州泰岳'
      }],
      departmentList: [{
        value: 10101,
        label: '行政人力'
      }, {
        value: 10202,
        label: '法务部'
      }],
      gridData: [{
        name: '姜婷',
        time: '2017-012-02',
        amount: '1'
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
        name: '联想笔记本',
        model: 'TP125',
        amount: '2',
        company: '泰岳梧桐',
        department: '财务部',
        buy_time: '2017-08-01'
      }, {
        id: 2,
        name: '苹果笔记本',
        model: '顶配2016款',
        amount: 1,
        company: '泰岳梧桐',
        department: '法务部',
        buy_time: '2018-03-01'
      }]

      this.listLoading = false
    },
    createAssets() {
      this.dialogTitle = '新增资产'
      this.form = {
        name: '',
        tax_code: ''
      }
      this.dialogFormVisible = true
    },
    updateAssets(row) {
      this.dialogTitle = '修改资产详情'
      this.form = row
      this.dialogFormVisible = true
    },
    receiveAssets(row) {

    },
    showAssets(row) {

    }
  }
}
</script>
