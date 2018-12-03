<template>
  <div class="dashboard-container">
    <h3 class="title">留学生报名系统管理端</h3>
    <ul>
      <li class="card-panel" @click="gotoExamine()">
        <div class="card-panel-icon-wrapper icon-message">
          <svg-icon icon-class="edit" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">待审批材料</div>
          <span class="card-panel-num">222<span>人</span></span>
        </div>
      </li>
      <li class="card-panel">
        <div class="card-panel-icon-wrapper icon-money">
          <svg-icon icon-class="money" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">待审批缴费</div>
          <span class="card-panel-num">222<span>人</span></span>
        </div>
      </li>
      <li class="card-panel">
        <div class="card-panel-icon-wrapper icon-pass">
          <svg-icon icon-class="pass" class-name="card-panel-icon" />
        </div>
        <div class="card-panel-description">
          <div class="card-panel-text">审批通过</div>
          <span class="card-panel-num">222<span>人</span></span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'Dashboard',
  data() {

  },
  methods: {
    gotoExamine() {
      this.$router.push({ path: '/' })
    }
  },
  computed: {
    ...mapGetters([
      'name',
      'roles'
    ])
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
    ul{
      // height: 150px;
      display: flex;
      flex-direction: row;
      justify-content: space-around;
      .card-panel{
        width: 29%;
        border-radius: 10px;
        list-style:none;
        background-color: #fff;
        float: left;
        border-color: rgba(0,0,0,.05);
        box-shadow: 4px 4px 40px rgba(0,0,0,.05);
        color: #666;
        cursor: pointer;
        font-size: 12px;
        height: 108px;
        &:hover{
          .card-panel-icon-wrapper {
            color: #fff;
          }
          .icon-message {
            background: #36a3f7;
          }
          .icon-money {
            background: #F29700;
          }
          .icon-pass {
            background: #34bfa3
          }
        }
        .icon-message {
          color: #36a3f7;
        }
        .icon-money {
          color: #F29700;
        }
        .icon-pass {
          color: #34bfa3
        }
        .card-panel-icon-wrapper {
          float: left;
          margin: 14px 0 0 14px;
          padding: 16px;
          transition: all 0.38s ease-out;
          border-radius: 6px;
        }
        .card-panel-icon {
          float: left;
          font-size: 48px;
        }
        .card-panel-description {
          float: right;
          font-weight: bold;
          margin: 26px;
          margin-left: 0px;
          padding-right:30px;
          .card-panel-text {
            line-height: 18px;
            color: rgba(0, 0, 0, 0.45);
            font-size: 16px;
            margin-bottom: 12px;
          }
          .card-panel-num {
            font-size: 20px;
          }
        }
      }
    }
  }

  &-text {
    font-size: 30px;
    line-height: 46px;
  }

}

</style>
