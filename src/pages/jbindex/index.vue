<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">

    <div class="row  p-2 justify-content-center align-items-center">
      <!--<a class="d-block btn text-white" :href="'/pages/jbdashboard/main'" role="button" style="background-color: #D78F30">-->
        <!--Start a New Ventures1-->
      <!--</a>-->
    </div>

    <template v-for="project in projects">
      <swiper :style="{height:'81px'}">
        <swiper-item>
          <li>
            <div class="weui-btn-area">
              <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbdashboard/main?projectId='+project.exProjectId+'&userRoleId='+project.userRole ">{{project.name}}</a>
            </div>
          </li>
        </swiper-item>
        <swiper-item v-if="roleId==1" style="margin-top:1px;">
          <li>
            <div class="row pl-3 pt-3">
              <div class="col-7">
                <a class="pt-3 btn btn-lg text-white btn-block btn-primary px-0" :href="'/pages/jbdashboard/main?projectId='+project.exProjectId+'&userRoleId='+project.userRole ">{{project.name}}</a>
              </div>

               <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white  btn-primary" :href="'/pages/jbindex-manage-project/main?projectId='+project.exProjectId">修改</a>
              </div>

              <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white btn-primary" @click="deleteProject(project.exProjectId)">删除</a>
              </div>

            </div>
          </li>
        </swiper-item>
      </swiper>
    </template>

    <template v-if="number==0">
      <div v-if="roleId==1" class="fixed-top"  style="height: 90vh">
        <div class="d-flex h-100 flex-column justify-content-center align-items-center">
          <img src="/static/images/icon/notification.png" class="pb-3" style="width: 4rem; height: 4rem">
          <p class="font-weight-bold">还没有创建项目</p>
          <p class="small w-50 text-center font-weight-light">请先创建一个项目，所有创建的项目将会显示在这里。</p>

          <a href="/pages/jbindex-manage-project/main" class="btn btn-primary text-white w-25 mt-5">创建项目</a>
        </div>
      </div>
    </template>
    <template v-else-if="number>0">
      <div v-if="roleId==1" class="row pl-3 pt-3">
        <div class="col-10 d-flex h-100 flex-column justify-content-center align-items-center">
          <a href="/pages/jbindex-manage-project/main" class="btn btn-primary text-white w-25 mt-5">创建项目</a>
        </div>
      </div>
    </template>

  </div>

</template>

<script>
import api from '@/api/index'

export default {
  data() {
    return {
      projects: [],
      number:0,
      roleId:0
    }
  },
  onShow() {
    let roleId = wx.getStorageSync('roleId')
    this.roleId = roleId

    this.listProjects()
  },
  mounted() {
//    this.listProjects()
  },
  methods: {
    listProjects() {
      console.log("wx.getStorageSync('userId'):"+wx.getStorageSync('userId'))
      let data = {
        userId:wx.getStorageSync('userId')
      }
      // 获取项目列表接口
      api.get('/listProjects.do',data).then(response => {
        //console.log(JSON.stringify(response))
        //var result = response.result
        console.log("projects number:"+response[1][0])
        console.log(JSON.stringify(response))

        this.number = response[1][0]
        this.projects = response[2]
        //console.log(result['kie-containers']['kie-container'][0]['container-id'])
      }).catch(error => {
        console.log(error)
      })
    },

    deleteProject(pid) {
      console.log('pid='+pid)
      wx.showModal({
        title: '提示',
        content: '确定要删除吗？',
        success: function (sm) {
          if (sm.confirm) {
            // 用户点击了确定 可以调用删除方法了
            let data = {
              projectId:pid
            }
            // 获取项目列表接口
            api.get('/deleteProject.do',data).then(response => {
              //console.log(JSON.stringify(response))
              //var result = response.result
              wx.redirectTo({
                url: '/pages/jbindex/main'
              })
            }).catch(error => {
              console.log(error)
            })
          } else if (sm.cancel) {
            console.log('用户点击取消')
          }
        }
      })
    }

  }
}

</script>

<style>

</style>
