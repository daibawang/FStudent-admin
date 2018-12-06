<template >
  <div class="stayApproval_container">
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" inline class="SA-table-expand">
            <el-form-item label="宗教信仰" style="margin-bottom:10px">
              <span>{{ props.row.religion }}</span>
            </el-form-item>
            <el-form-item label="英语成绩" style="margin-bottom:10px">
              <span>{{ props.row.level_e }}</span>
            </el-form-item>
            <el-form ref="Achievements" :inline="true" :model="props.row.Achievements" class="demo-dynamic" style="margin-bottom:15px">
              <div v-for="(item, index) in props.row.Xs" :key="index.username">
                <el-form-item
                  :prop="'Xs.'+index+'.papers'"
                  label="论文题目"
                  style="margin-top:10px;">
                  <el-input v-model="item.papers" readonly style="width:320px;" clearable />
                </el-form-item>
                <el-form-item :prop="'Xs.'+index+'.time'" label="发表时间" style="margin-top:10px;">
                  <el-date-picker v-model="item.time" readonly style="width:320px;" value-format="yyyyMM" type="month" placeholder="from" />
                </el-form-item>
              </div>
            </el-form>
            <el-form ref="WorkForm" :inline="true" :model="props.row.WorkForm" class="demo-dynamic">
              <div v-for="(item, index) in props.row.Work" :key="index.username" class="WorkForm_border">
                <el-form-item label="工作时间" style="margin-top:20px;" >
                  <el-col :span="11">
                    <el-form-item :prop="'Work.'+index+'.btime'">
                      <el-date-picker v-model="item.btime" readonly style="width:130px;" value-format="yyyyMM" type="month" placeholder="from"/>
                    </el-form-item>
                  </el-col>
                  <el-col :span="11">
                    <el-form-item :prop="'Work.'+index+'.ltime'">
                      <el-date-picker v-model="item.ltime" readonly value-format="yyyyMM" style="width:130px;margin-left:30px;" type="month" placeholder="to" />
                    </el-form-item>
                  </el-col>
                </el-form-item>
                <el-form-item
                  :prop="'Work.'+index+'.unit'"
                  style="margin-top:20px;"
                  label="工作单位"
                  class="">
                  <el-input v-model="item.unit" readonly clearable style="width:320px;" />
                </el-form-item>
                <el-form-item :prop="'Work.'+index+'.obj'" style="margin-top:10px;" label="工作职位">
                  <el-input v-model="item.obj" readonly clearable style="width:320px;" />
                </el-form-item>
              </div>
            </el-form>
          </el-form>
        </template>
      </el-table-column>
      <el-table-column
        label="姓名"
        prop="name"/>
      <el-table-column
        label="生日"
        prop="date_birth"/>
      <el-table-column
        label="国家"
        prop="nationality"/>
      <el-table-column
        label="毕业院校"
        prop="unit"/>
      <el-table-column
        label="专业"
        prop="obj"/>
      <el-table-column label="操作" width="250">
        <template slot-scope="scope">
          <a href="http://www.istuadmission.com/FStudent/Attachment_Overseas_Remittance_ofForeign_Exchange.zip">
            <el-button
              size="small"
              type="primary"
              icon="el-icon-download">下载</el-button>
          </a>
          <el-button
            style="margin-left:7px"
            size="small"
            type="success"
            @click="handlepass(scope.$index,scope.row)">通过</el-button>
          <el-button
            size="small"
            type="danger"
            @click="handleEdit(scope.$index, scope.row)">不通过</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <div class="Pagination">
      <el-pagination
        :current-page="currentPage"
        :page-size="8"
        :total="count"
        background
        layout="total, prev, pager, next"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"/>
    </div>
    <el-dialog :visible.sync="dialogFormVisible" title="审批意见">
      <el-input
        :autosize="{ minRows: 3, maxRows: 4 }"
        v-model="approvalInput"
        type="textarea"
        placeholder="请输入审批意见"/>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="addapproval">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
export default{
  data() {
    return {
      dialogFormVisible: false,
      approvalInput: '',
      dialogIndex: '',
      currentPage: 1,
      offset: 0,
      limit: 8,
      count: null,
      changusername: '',
      tableData: [{
        papersExport: '',
        workExport: ''
      }
      ]
    }
  },
  created: function() {
    this.initData()
  },
  methods: {
    initData() {
      this.$axios.get(this.$URL + 'GetCountServlet', {
        params: {}
      }).then((response) => {
        this.count = parseInt(response.data)
        this.getRecord()
      }).catch(() => {
      })
    },
    handlepass(index, row) {
      // console.log(index)
      this.$confirm('确定通过这条记录吗？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消'
      }).then(() => {
        this.$axios.get(this.$URL + 'ChangExServlet', {
          params: {
            username: row.username,
            state: '审核通过'
          }
        }).then((response) => {
          console.log(response.data)
          if (response.data == true) {
            this.getRecord()
          }
          this.$alert('已通过该条申请', {
            confirmButtonText: 'sure'
          })
        }).catch(() => {
        })
      }).catch(() => {

        // this.$message({
        //   type: 'info',
        //   message: '已取消删除'
        // })
      })
    },
    // 不通过
    handleEdit(index, row) {
      this.dialogIndex = row.username
      this.dialogFormVisible = true
      this.changusername = row.username
    },
    addapproval() {
      const thistime = new Date().getTime()
      this.dialogFormVisible = false
      this.$axios.get(this.$URL + 'NoPassServlet', {
        params: {
          username: this.changusername,
          massage: this.approvalInput,
          time: thistime
        }
      }).then((response) => {
        console.log(response.data)
        if (response.data == true) {
          this.getRecord()
        } else {
          this.$alert('审核失败', {
            confirmButtonText: 'sure'
          })
        }
      }).catch(() => {

      })
    },
    // 分页
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      this.currentPage = val // 当前页数
      this.offset = (val - 1) * this.limit // 第几条开始
      this.getRecord()
    },
    getRecord() {
      this.$axios.get(this.$URL + 'ExclAllServlet', {
        params: {
          firstL: this.offset,
          lastL: this.limit,
          typ: 12
        }
      }).then((response) => {
        this.tableData = response.data
        for (var i = 0; i < this.tableData.length; i++) {
          this.tableData[i].date_birth = response.data[i].date_birth.substr(0, 4) + '-' + response.data[i].date_birth.substr(4, 2) + '-' + response.data[i].date_birth.substr(6, 2)
          // this.tableData[i].papersExport = ''
          // for (var j = 0; j < response.data[i].Xs.length; j++) {
          //   this.tableData[i].papersExport += (j + 1) + '、' + response.data[i].Xs[j].papers + '/' + response.data[i].Xs[j].time.substr(0, 4) + '年' + response.data[i].Xs[j].time.substr(4, 2) + '月' + '\n'
          // }
          // this.tableData[i].workExport = ''
          // for (var t = 0; t < response.data[i].Work.length; t++) {
          //   this.tableData[i].workExport += (t + 1) + '、' + response.data[i].Work[t].btime + '-' + response.data[i].Work[t].ltime + '单位' + response.data[i].Work[t].unit + '职位：' + response.data[i].Work[t].obj + '\n'
          // }
          // j = 0
          // t = 0
        }
      }).catch(() => {

      })
    }
  }
}

</script>
<style rel="stylesheet/scss" lang="scss" scoped>
.stayApproval_container{
  padding: 30px;
  .SA-table-expand{
    font-size: 0;
    label{
      width: 90px;
      color: #99a9bf;
    }
    .el-form-item{
      margin-right: 0;
      margin-bottom: 0;
      width: 50%;
    }
  }
  .Pagination{
    display: flex;
    justify-content: flex-start;
    margin-top: 8px;
  }
}
</style>
