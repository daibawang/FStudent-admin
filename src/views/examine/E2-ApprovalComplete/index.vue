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
              <span>{{ props.row.englishscore }}</span>
            </el-form-item>
            <el-form ref="Achievements" :inline="true" :model="props.row.Achievements" class="demo-dynamic" style="margin-bottom:15px">
              <div v-for="(item, index) in props.row.Achievements.domains" :key="i">
                <el-form-item
                  :prop="'domains.'+index+'.papers'"
                  label="论文题目"
                  style="margin-top:10px;">
                  <el-input v-model="item.papers" readonly style="width:320px;" clearable />
                </el-form-item>
                <el-form-item :prop="'domains.'+index+'.time'" label="发表时间" style="margin-top:10px;">
                  <el-date-picker v-model="item.time" readonly style="width:320px;" value-format="yyyyMM" type="month" placeholder="from" />
                </el-form-item>
              </div>
            </el-form>
            <el-form ref="WorkForm" :inline="true" :model="props.row.WorkForm" class="demo-dynamic">
              <div v-for="(item, index) in props.row.WorkForm.domains" :key="j" class="WorkForm_border">
                <el-form-item label="工作时间" style="margin-top:20px;" >
                  <el-col :span="11">
                    <el-form-item :prop="'domains.'+index+'.btime'">
                      <el-date-picker v-model="item.btime" readonly style="width:130px;" value-format="yyyyMM" type="month" placeholder="from"/>
                    </el-form-item>
                  </el-col>
                  <el-col :span="11">
                    <el-form-item :prop="'domains.'+index+'.ltime'">
                      <el-date-picker v-model="item.ltime" readonly value-format="yyyyMM" style="width:130px;margin-left:30px;" type="month" placeholder="to" />
                    </el-form-item>
                  </el-col>
                </el-form-item>
                <el-form-item
                  :prop="'domains.'+index+'.unit'"
                  style="margin-top:20px;"
                  label="工作单位"
                  class="">
                  <el-input v-model="item.unit" readonly clearable style="width:320px;" />
                </el-form-item>
                <el-form-item :prop="'domains.'+index+'.obj'" style="margin-top:10px;" label="工作职位">
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
        prop="studyschool"/>
      <el-table-column
        label="专业"
        prop="fields"/>
      <el-table-column label="操作" width="0">
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
        :page-size="3"
        :total="count"
        background
        layout="total, prev, pager, next"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"/>
    </div>
  </div>
</template>
<script>
import FilenameOption from '../components/FilenameOption'
import BookTypeOption from '../components/BookTypeOption'
export default{
  components: { FilenameOption, BookTypeOption },
  data() {
    return {
      approvalInput: '',
      dialogIndex: '',
      currentPage: 1,
      limit: 3,
      count: 10,
      filename: 'excel-list',
      autoWidth: true,
      bookType: 'xlsx',
      downloadLoading: false,
      tableData: [{
        date: '2016-05-02',
        username: 'zmj1',
        name: '王小虎',
        date_birth: '19971130',
        religion: '佛教',
        nationality: '中国',
        studyschool: '北京城市学院',
        fields: '软件工程',
        englishscore: 'TOEFL666',
        papersExport: '',
        workExport: '',
        Achievements: {
          domains: [{
            time: '201809',
            papers: '一个信仰1111111'
          }, {
            time: '201809',
            papers: '一个信仰11111111'
          }]
        },
        WorkForm: {
          domains: [{
            unit: '北京城市学院北京城市学院北京城市学院北京城市学院北京城市学院',
            btime: '201709',
            ltime: '201809',
            obj: '吱吱吱吱开发'
          }, {
            unit: '北京城市学院北京城市学院北京城市学院北京城市学院北京城市学院',
            btime: '201709',
            ltime: '201809',
            obj: '吱吱吱吱开发'
          }]
        }
      },
      {
        date: '2016-05-02',
        name: '王小虎',
        username: 'zmj2',
        date_birth: '19971130',
        religion: '佛教',
        nationality: '中国',
        studyschool: '北京城市学院',
        fields: '软件工程',
        englishscore: 'TOEFL666',
        Achievements: {
          domains: [{
            time: '201809',
            papers: '一个信仰'
          }, {
            time: '201809',
            papers: '一个信仰'
          }]
        },
        WorkForm: {
          domains: [{
            unit: '北京城市学院北京城市学院北京城市学院北京城市学院北京城市学院',
            btime: '201709',
            ltime: '201809',
            obj: '吱吱吱吱开发'
          }, {
            unit: '北京城市学院北京城市学院北京城市学院北京城市学院北京城市学院',
            btime: '201709',
            ltime: '201809',
            obj: '吱吱吱吱开发'
          }]
        }
      }
      ]
    }
  },
  created: function() {
    // this.count = 4
  },
  methods: {
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['姓名', '生日', '宗教信仰', '国家', '毕业院校', '作业', '英语成绩', '论文', '工作经历'] // 对应表格输出的title
        const filterVal = ['name', 'date_birth', 'religion', 'nationality', 'studyschool', 'fields', 'englishscore', 'Achievements.domains', 'WorkForm.domains.workput'] // 对应表格输出的数据
        const list = this.tableData
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
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
    },
    handlepass(index, row) {
      // console.log(index)
      // console.log(row.username)
      this.$confirm('确定通过这条记录吗？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消'
      }).then(() => {
        // this.specs.splice(index, 1);
        console.log(row.username)
      }).catch(() => {
        // this.$message({
        //   type: 'info',
        //   message: '已取消删除'
        // })
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
      this.$axios.get(this.$URL + '', {
        params: {
          offset: this.offset,
          limit: this.limit
        }
      }).then((response) => {

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
