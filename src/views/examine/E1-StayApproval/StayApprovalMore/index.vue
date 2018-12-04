<template >
  <div class="stayApproval_container">
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" inline class="SA-table-expand">
            <el-form-item label="审批总次数" style="color:#2F88EE">
              <span>{{ props.row.count }}次</span>
            </el-form-item>
            <el-form-item label="上次审批时间" style="color:#2F88EE">
              <span>{{ props.row.approvalTime }}</span>
            </el-form-item>
            <el-form-item label="上次审批意见" style="width:83%;text-align: justify;color:#2F88EE">
              <span style="">{{ props.row.message }}</span>
            </el-form-item>
            <el-form-item label="宗教信仰" style="margin-top:10px;margin-bottom:10px">
              <span>{{ props.row.religion }}</span>
            </el-form-item>
            <el-form-item label="英语成绩"style="margin-top:10px;margin-bottom:10px">
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
        :page-size="3"
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
      limit: 3,
      count: 10,
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
        count: '5',
        approvalTime: '2018-10-11',
        message: `通过设置 type="expand" 和 Scoped slot 可以开启展开行功能，
        el-table-column 的模板会被渲染成为展开板时Scoped slot 相同。的 Scoped slot 相同通过设置 type="expand" 和 Scoped slot 可以开启展开行功能，
        el-table-column 的模板会被渲染成为展开板时Scoped slot 相同。的 Scoped slot 相同通过设置 type="expand" 和 Scoped slot 可以开启展开行功能，
        el-table-column 的模板会被渲染成为展开板时Scoped slot 相同。的 Scoped slot 相同.`,
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
        },
        works: '201809-201909北京城市学院11111',
        address: '上海市普陀区金沙江路 1518 弄11111'
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
        },
        works: '201809-201909北京城市学院',
        address: '上海市普陀区金沙江路 1518 弄'
      },
      {
        date: '2016-05-02',
        name: '王小虎',
        username: 'zmj3',
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
        },
        works: '201809-201909北京城市学院',
        address: '上海市普陀区金沙江路 1518 弄'
      },
      {
        date: '2016-05-02',
        name: '王小虎',
        date_birth: '19971130',
        religion: '佛教',
        username: 'zmj4',
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
        },
        works: '201809-201909北京城市学院',
        address: '上海市普陀区金沙江路 1518 弄'
      }

      ]
    }
  },
  methods: {
    handlepass(index, row) {
      // console.log(index)
      // console.log(row.username)
      this.$confirm('确定通过这条记录吗？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消'
      }).then(() => {
        console.log(row.username)
      }).catch(() => {
        // this.$message({
        //   type: 'info',
        //   message: '已取消删除'
        // })
      })
    },
    addapproval() {
      console.log('dialogForm' + this.dialogIndex)
      this.dialogFormVisible = false
    },
    handleEdit(index, row) {
      this.dialogIndex = row.username
      this.dialogFormVisible = true
    },
    // 分页
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      this.currentPage = val
      this.offset = (val - 1) * this.limit
      this.getFoods()
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
      width: 100px;
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
