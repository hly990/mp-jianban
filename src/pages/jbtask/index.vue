<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">

    <div class="row  p-2 justify-content-center align-items-center">

    </div>

    <template v-for="task in tasks">
      <swiper :style="{height:'81px'}">
        <swiper-item>
          <li>
            <div class="weui-btn-area">
              <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbtask-detail/main?taskId='+task.exWorkFlowInstanceId">{{task.name}}</a>
            </div>
          </li>
        </swiper-item>
        <swiper-item v-if="roleId==1" style="margin-top:1px;">
          <li>
            <div class="row pl-3 pt-3">
              <div class="col-7">
                <a class="pt-3 btn btn-lg text-white btn-block btn-primary px-0" :href="'/pages/jbtask-detail/main?taskId='+task.exWorkFlowInstanceId">{{task.name}}</a>
              </div>

               <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white  btn-primary" :href="'/pages/jbtask-manage/main?taskId='+task.exWorkFlowInstanceId">修改</a>
              </div>

              <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white btn-primary" @click="deleteTask(task.exWorkFlowInstanceId,task.parentId)">删除</a>
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
          <p class="font-weight-bold">还没有创建任务</p>
          <p class="small w-50 text-center font-weight-light">请先创建一个任务，所有创建的任务将会显示在这里。</p>

          <a :href="'/pages/jbtask-manage/main?parentId='+parentId" class="btn btn-primary text-white w-25 mt-5">创建任务</a>
        </div>
      </div>
    </template>
    <template v-else-if="number>0">
      <div v-if="roleId==1" class="row pl-3 pt-3">
        <div class="col-10 d-flex h-100 flex-column justify-content-center align-items-center">
          <a :href="'/pages/jbtask-manage/main?parentId='+parentId" class="btn btn-primary text-white w-25 mt-5">创建任务</a>
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
      tasks: [],
      parentId:'',
      roleId:0,
      number:0
    }
  },

  onShow() {
    this.getParams()
    this.listTasks()
  },

  onUnload() {
    console.log("getCurrentPages().length=="+getCurrentPages().length)
//    if(getCurrentPages().length > 4) {
//      wx.navigateBack({
//        delta: 2
//      })
//    }
  },

  mounted() {
//    this.listTasks()
  },
  methods: {
    listTasks() {
      console.log("parentId:"+this.parentId)
      let data = {
        parentId:this.parentId
      }
      // 获取项目列表接口
      api.get('/listTasks.do',data).then(response => {
        //console.log(JSON.stringify(response))
        //var result = response.result
        console.log("projects number:"+response[1][0])
        console.log(JSON.stringify(response))

        this.number = response[1][0]
        this.tasks = response[2]
        //console.log(result['kie-containers']['kie-container'][0]['container-id'])
      }).catch(error => {
        console.log(error)
      })
    },

    deleteTask(taskId,parentId) {
      console.log('taskId='+taskId)
      wx.showModal({
        title: '提示',
        content: '确定要删除吗？',
        success: function (sm) {
          if (sm.confirm) {
            // 用户点击了确定 可以调用删除方法了
            let data = {
              taskId:taskId
            }
            // 获取项目列表接口
            api.get('/deleteTask.do',data).then(response => {
              //console.log(JSON.stringify(response))
              //var result = response.result

              console.log("deleteTask parentId=="+parentId)

//              wx.navigateTo({
//                url: '/pages/jbtask/main?parentId='+parentId
//              })
              wx.redirectTo({
                url: '/pages/jbtask/main?parentId='+parentId
              })



            }).catch(error => {
              console.log(error)
            })
          } else if (sm.cancel) {
            console.log('用户点击取消')
          }
        }
      })
    },

    getParams(){

      if (this.$root.$mp.query && this.$root.$mp.query.parentId) {
        console.log("parentId=="+this.$root.$mp.query.parentId)
        this.parentId = this.$root.$mp.query.parentId
      }
      let roleId = wx.getStorageSync('roleId')
      this.roleId = roleId
    }

  }
}

</script>

<style>

</style>
