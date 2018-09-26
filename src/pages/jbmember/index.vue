<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">

    <div class="row  p-2 justify-content-center align-items-center">
      <!--<a class="d-block btn text-white" :href="'/pages/jbdashboard/main'" role="button" style="background-color: #D78F30">-->
        <!--Start a New Ventures1-->
      <!--</a>-->
    </div>

    <template v-for="user in users">
      <swiper :style="{height:'81px'}">
        <swiper-item>
          <li>
            <div class="weui-btn-area">
              <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbmember-manage/main?userId='+user.exUserId">{{user.des}}</a>
            </div>
          </li>
        </swiper-item>
        <swiper-item style="margin-top:1px;">
          <li>
            <div class="row pl-3 pt-3">
              <div class="col-7">
                <a class="pt-3 btn btn-lg text-white btn-block btn-primary px-0" :href="'/pages/jbmember-manage/main?userId='+user.exUserId">{{user.des}}</a>
              </div>

               <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white  btn-primary" :href="'/pages/jbmember-manage/main?userId='+user.exUserId">修改</a>
              </div>

              <div class="col-1" :style="{padding: '10px'}">
                <a class="p-1 btn btn-sm text-white btn-primary" @click="deleteUser(user.exUserId)">删除</a>
              </div>

            </div>
          </li>
        </swiper-item>
      </swiper>
    </template>

    <template v-if="number==0">
      <div class="fixed-top"  style="height: 90vh">
        <div class="d-flex h-100 flex-column justify-content-center align-items-center">
          <img src="/static/images/icon/notification.png" class="pb-3" style="width: 4rem; height: 4rem">
          <p class="font-weight-bold">还没有添加项目成员</p>
          <p class="small w-50 text-center font-weight-light">请添加项目成员，所有添加的成员将会显示在这里。</p>

          <a href="/pages/jbmember-manage/main" class="btn btn-primary text-white w-25 mt-5">添加</a>
        </div>
      </div>
    </template>
    <template v-else-if="number>0">
      <div class="row pl-3 pt-3">
        <div class="col-10 d-flex h-100 flex-column justify-content-center align-items-center">
          <a href="/pages/jbmember-manage/main" class="btn btn-primary text-white w-25 mt-5">添加</a>
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
      users: [],
      number:0
    }
  },
  onShow() {
    this.listUsers()
  },
  mounted() {
//    this.listProjects()
  },
  methods: {
    listUsers() {
      console.log("wx.getStorageSync('projectId'):"+wx.getStorageSync('projectId'))
      let data = {
        projectId:wx.getStorageSync('projectId')
      }
      // 获取项目列表接口
      api.get('/listUsers.do',data).then(response => {
        //console.log(JSON.stringify(response))
        //var result = response.result
        console.log(JSON.stringify(response))

        this.number = response[1][0]
        this.users = response[2]
        //console.log(result['kie-containers']['kie-container'][0]['container-id'])
      }).catch(error => {
        console.log(error)
      })
    },

    deleteUser(exUserId) {
      wx.showModal({
        title: '提示',
        content: '确定要删除吗？',
        success: function (sm) {
          if (sm.confirm) {
            // 用户点击了确定 可以调用删除方法了
            let data = {
              exUserId:exUserId
            }
            // 获取项目列表接口
            api.get('/deleteUser.do',data).then(response => {
              //console.log(JSON.stringify(response))
              //var result = response.result
              wx.redirectTo({
                url: '/pages/jbmember/main'
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
