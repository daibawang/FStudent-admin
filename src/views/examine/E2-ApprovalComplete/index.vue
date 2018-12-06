<template >
  <div class="stayApproval_container">
    <div>
      <FilenameOption v-model="filename" />
      <BookTypeOption v-model="bookType" />
      <el-button :loading="downloadLoading" style="margin:0 0 20px 50px;" type="primary" icon="document" @click="handleDownload">导出 Excel</el-button>
    </div>
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
      <el-table-column label="操作" width="90">
        <template slot-scope="scope">
          <a href="http://www.istuadmission.com/FStudent/Attachment_Overseas_Remittance_ofForeign_Exchange.zip">
            <el-button
              size="small"
              type="primary"
              icon="el-icon-download">下载</el-button>
          </a>
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
  </div>
</template>
<script>
import FilenameOption from '../../../components/FilenameOption'
import BookTypeOption from '../../../components/BookTypeOption'
export default{
  components: { FilenameOption, BookTypeOption },
  data() {
    return {
      approvalInput: '',
      dialogIndex: '',
      currentPage: 1,
      offset: 0,
      limit: 8,
      count: null,
      filename: 'excel-list',
      autoWidth: true,
      bookType: 'xlsx',
      downloadLoading: false,
      tableData: [],
      tableDataAll: []
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
    handleDownload() {
      this.$axios.get(this.$URL + 'ExclAllServlet', {
        params: {
          firstL: 0,
          lastL: this.count
        }
      }).then((response) => {
        this.tableDataAll = response.data
        for (var i = 0; i < this.tableDataAll.length; i++) {
          this.tableDataAll[i].date_birth = response.data[i].date_birth.substr(0, 4) + '-' + response.data[i].date_birth.substr(4, 2) + '-' + response.data[i].date_birth.substr(6, 2)
          this.tableDataAll[i].papersExport = ''
          for (var j = 0; j < response.data[i].Xs.length; j++) {
            this.tableDataAll[i].papersExport += (j + 1) + '、' + response.data[i].Xs[j].papers + '/' + response.data[i].Xs[j].time.substr(0, 4) + '年' + response.data[i].Xs[j].time.substr(4, 2) + '月' + '  '
          }
          this.tableDataAll[i].workExport = ''
          for (var t = 0; t < response.data[i].Work.length; t++) {
            this.tableDataAll[i].workExport += (t + 1) + '、' + response.data[i].Work[t].btime + '-' + response.data[i].Work[t].ltime + '单位' + response.data[i].Work[t].unit + '职位：' + response.data[i].Work[t].obj + '  '
          }
          j = 0
          t = 0
        }
        this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['姓名', '生日', '宗教信仰', '国家', '毕业院校', '作业', '英语成绩', '论文', '工作经历'] // 对应表格输出的title
          const filterVal = ['name', 'date_birth', 'religion', 'nationality', 'unit', 'obj', 'level_e', 'papersExport', 'workExport'] // 对应表格输出的数据
          const list = this.tableDataAll
          const data = this.formatJson(filterVal, list)
          excel.export_json_to_excel({
            header: tHeader,
            data,
            filename: this.filename,
            autoWidth: this.autoWidth,
            bookType: this.bookType
          })
          this.downloadLoading = false
        })
      }).catch(() => {
      })
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
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
          lastL: this.limit
        }
      }).then((response) => {
        this.tableData = response.data
        for (var i = 0; i < this.tableData.length; i++) {
          this.tableData[i].date_birth = response.data[i].date_birth.substr(0, 4) + '-' + response.data[i].date_birth.substr(4, 2) + '-' + response.data[i].date_birth.substr(6, 2)
          this.tableData[i].papersExport = ''
          for (var j = 0; j < response.data[i].Xs.length; j++) {
            this.tableData[i].papersExport += (j + 1) + '、' + response.data[i].Xs[j].papers + '/' + response.data[i].Xs[j].time.substr(0, 4) + '年' + response.data[i].Xs[j].time.substr(4, 2) + '月' + '  '
          }
          this.tableData[i].workExport = ''
          for (var t = 0; t < response.data[i].Work.length; t++) {
            this.tableData[i].workExport += (t + 1) + '、' + response.data[i].Work[t].btime + '-' + response.data[i].Work[t].ltime + '单位' + response.data[i].Work[t].unit + '职位：' + response.data[i].Work[t].obj + '  '
          }
          j = 0
          t = 0
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
