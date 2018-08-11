<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">
    <!--section 1 -->
    <div class="row  pt-2 justify-content-center align-items-center">
        <h5>{{projectName}}</h5>
    </div>

    <div class="row">
      <div class="col-md-4 block">
        <div class="circle">
          <p>{{totalProgress}}%</p>
        </div>
        <p>项目总进度为{{totalProgress}}%</p>
      </div>
    </div>

    <!--section 2 -->
    <div class="row  pt-2 justify-content-center align-items-center">
      <h6>项目总进度表</h6>
    </div>

    <template v-for="processInstance in processInstances">
      <!--<a  :href="'/pages/jbtest-upload/main'">-->
      <a  @click="goProjectDetail(processInstance)">
        <div class="row p-1">
          <div class="col-4 text-right align-items-center">{{processInstance.processName}}</div>
          <template v-if="processInstance.processInstanceState==2">
            <div class="col-6 p-2 bg-success text-white"></div>
          </template>
          <template v-else>
            <div class="col-6 p-2 bg-secondary text-white"></div>
          </template>
        </div>
      </a>
    </template>

    <!--<a  :href="'/pages/jbtest-upload/main'">-->
      <!--<div class="row p-1">-->
          <!--<div class="col-4 text-right align-items-center">签订及复核文件</div>-->
          <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
      <!--</div>-->
    <!--</a>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">材料确认</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">现场符合</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">深化设计及图纸</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">定制材料备料</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">白胚</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">产品运输</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">产品现场安装</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">竣工验收</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--<div class="row p-1">-->
      <!--<div class="col-4 text-right align-items-center">售后服务</div>-->
      <!--<div class="col-6 p-2 bg-secondary text-white"></div>-->
    <!--</div>-->

    <!--section 3 -->
     <!--<div class="row p-3 justify-content-center align-items-center">-->
      <!--<h6>当前进度</h6>-->
      <!--<div class="col-md-4 block">-->
        <!--<div class="circle">-->
          <!--<p>50%</p>-->
        <!--</div>-->
        <!--<p>当前进度为材料确认</p>-->
      <!--</div>-->
    <!--</div>-->

  </div>

</template>

<script>
  import api from '@/api/index'

  export default {
    data() {
      return {
        processInstances: [],
        projectName: '',
        totalProgress: 0
      }
    },
    mounted() {
      this.listPrecessInstances()
      this.getParams()
      this.getTotalProgress()
    },
    methods: {
      listPrecessInstances() {
        // 获取项目列表接口
        api.get('/listPrecessInstances.do').then(response => {
          //console.log(JSON.stringify(response))
          //var result = response.result
          console.log(JSON.stringify(response))
          console.log(response[2])
          this.processInstances = response[2]
          //console.log(result['kie-containers']['kie-container'][0]['container-id'])
        }).catch(error => {
          console.log(error)
        })
      },

      getParams(){
        // 取到路由带过来的参数
        console.log(this.$root.$mp.query.projectName)
        var projectName = this.$root.$mp.query.projectName
        this.projectName = projectName
      },

      getTotalProgress() {
        // 获取完成百分比接口
        api.get('/totalProgress.do').then(response => {
          console.log(JSON.stringify(response))
          var entity = response[1]
          console.log(entity)
          this.totalProgress = entity.percentNum
        }).catch(error => {
          console.log(error)
        })
      },

      goProjectDetail(processInstance) {
        console.log("goProjectDetail processInstance.processInstanceId=="+processInstance.processInstanceId)
        wx.setStorageSync('processInstanceId', processInstance.processInstanceId)
//        wx.setStorageSync('loginname', user.password)
//        wx.setStorageSync('roleId', user.roleId)
        wx.navigateTo({
          url: '/pages/jbtest-upload/main'
        })
      }

    }
  }

</script>

<style>
  .block {
    border: 0px solid #95BBDD;
    text-align: center;
    vertical-align: middle;
  }
  .circle {
    background: #95BBDD;
    border-radius: 95px;
    color: white;
    height: 95px;
    font-weight: bold;
    width: 95px;
    display: table;
    margin: 10px auto;
  }
  .circle p {
    vertical-align: middle;
    display: table-cell;
  }

</style>
