<template>
  <div class="dashboard-container">
    <h3 class="title">留学生报名系统开放时间设置</h3>
    <el-form ref="form" :model="form" label-width="80px" style="width:35%;margin:50px auto">
      <el-form-item label="开始报名">
        <el-date-picker v-model="form.date1" value-format="yyyyMMdd" type="date" placeholder="选择日期" style="width: 100%;"/>
      </el-form-item>
      <el-form-item label="截止报名">
        <el-date-picker v-model="form.date2" value-format="yyyyMMdd" type="date" placeholder="选择日期" style="width: 100%;"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width:150px;margin:20px auto" @click="onSubmit">确定</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
// import { mapGetters } from 'vuex'
export default {
  name: 'Dashboard',
  data() {
    return {
      form: {
        date1: '',
        date2: ''
      }
    }
  },
  computed: {
  },
  methods: {
    onSubmit() {

    }
  }
}
</script>
<style rel="stylesheet/scss" lang="scss" scoped>
.dashboard {
  &-container {
    padding-top: 20px;
    text-align: center;
    background-color: #EDEFF2;
    height: 400px;
    .title {
        font-size: 26px;
        font-weight: 400;
        color: #304156;
        text-align: center;
        font-weight: bold;
        letter-spacing: 3px;
    }

  }

  &-text {
    font-size: 30px;
    line-height: 46px;
  }

}

</style>
