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
          <div class="col-4 text-right align-items-center">{{processInstance.name}}</div>
            <template v-if="processInstance.processInstanceState==3">
              <div class="col-6 p-2  text-white" :style="{backgroundColor: '#FECD51'}"></div>
            </template>
            <template v-else-if="processInstance.processInstanceState==4">
              <div class="col-6 p-2  text-white" :style="{backgroundColor: '#F76074'}"></div>
            </template>
            <template v-else>
              <div class="col-6 p-2  text-white" :style="{backgroundColor:'#FFFFFF'}"></div>
            </template>
        </div>
      </a>
    </template>

    <!--<div class="row pl-3 pr-3 pt-3">-->
      <!--<div class="col-3 text-left align-items-center">-->
        <!--<span class="col-3 bg-secondary text-white"></span>-->
        <!--<span class="p-1" style="font-size:12px;">未开始</span>-->
      <!--</div>-->
      <!--<div class="col-3 text-left align-items-center">-->
        <!--<span class="col-3 bg-success text-white"></span>-->
        <!--<span class="p-1" style="font-size:12px;">执行中</span>-->
      <!--</div>-->
      <!--<div class="col-3 text-left align-items-center">-->
        <!--<span class="col-3 bg-primary text-white"></span>-->
        <!--<span class="p-1" style="font-size:12px;">已完成</span>-->
      <!--</div>-->
    <!--</div>-->

    <div class="row pl-3 pr-3 pt-1">
      <div class="col-3 text-left align-items-center">
        <span class="col-3  text-white" :style="{backgroundColor: '#FECD51'}"></span>
        <span class="p-1" style="font-size:12px;">即将到期</span>
      </div>
      <div class="col-3 text-left align-items-center">
        <span class="col-3  text-white" :style="{backgroundColor: '#F76074'}"></span>
        <span class="p-1" style="font-size:12px;">已过期</span>
      </div>
    </div>
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

    <div v-if="userRoleId==1" class="row pl-3 pt-3">
      <div class="col-10 d-flex h-100 flex-column justify-content-center align-items-center">
        <a href="/pages/jbmember/main" class="btn btn-primary text-white w-25 mt-5"><span style="font-size: 12px;">项目成员管理</span></a>
      </div>
    </div>

  </div>

</template>

<script>
  import api from '@/api/index'

  export default {
    data() {
      return {
        processInstances: [],
        projectId: '',
        userRoleId: 0,
        projectName: '',
        totalProgress: 0
      }
    },
    onShow() {
      this.getParams()
    },

    mounted() {
      this.listWorkFlowInstance()
      this.getTotalProgress()
    },
    methods: {
      listWorkFlowInstance() {
        let data = {
          projectId: this.projectId,
        }
        // 获取项目列表接口
        api.get('/listWorkFlowInstance.do',data).then(response => {
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
//        console.log("containerId=="+this.$root.$mp.query.containerId)
//        console.log("projectName=="+this.$root.$mp.query.projectName)
//
//        var containerId = this.$root.$mp.query.containerId
//        var projectName = this.$root.$mp.query.projectName
//
//        wx.setStorageSync('containerId', containerId)
//
//        this.containerId = containerId
//        this.projectName = projectName

        console.log("projectId=="+this.$root.$mp.query.projectId+"\r\n userRoleId=="+this.$root.$mp.query.userRoleId)
        this.projectId = this.$root.$mp.query.projectId
        this.userRoleId = this.$root.$mp.query.userRoleId

        wx.setStorageSync('projectId', this.projectId)
        wx.setStorageSync('userRoleId', this.userRoleId)


      },

      getTotalProgress() {
        let data = {
          projectId: this.projectId,
        }
        // 获取完成百分比接口
        api.get('/totalProgress.do',data).then(response => {
          console.log(JSON.stringify(response))
          var totalProgress = response[1]
          console.log("totalProgress="+totalProgress)
          this.totalProgress = totalProgress
        }).catch(error => {
          console.log(error)
        })
      },

      goProjectDetail(processInstance) {
//        console.log("goProjectDetail processInstance.processInstanceId=="+processInstance.processInstanceId)
//        wx.setStorageSync('processInstanceId', processInstance.processInstanceId)
//        wx.navigateTo({
//          url: '/pages/jbtest-upload/main'
//        })

        wx.navigateTo({
          url: '/pages/jbtask/main?parentId='+processInstance.exWorkFlowInstanceId
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
