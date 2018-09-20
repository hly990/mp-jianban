<template>
  <div class="page">
    <!-- 指向微信公众平台首页的web-view -->

    <!--<web-view src="http://127.0.0.1/mp-jianban-web/jianban-form2.html?roleId=1&processInstanceId={{processInstanceId}}"></web-view>-->
    <web-view :src="url"></web-view>
    <!--<web-view src="https://dmodev.cn/jianban-form1.html"></web-view>-->
  </div>

</template>


<script>
  import store from '@/pages/counter/store'
  export default {
      data() {
        return {
          url: '',
          containerId: '',
          processInstanceId: ''
        }
      },
      onShow() {
        this.getProcessInstanceId()
      },
      mounted() {

        this.loadPage()

//        this.url= ''
//        this.processInstanceId=''
//        let  processInstanceId = wx.getStorageSync('processInstanceId')
//        let url = 'http://127.0.0.1/mp-jianban-web/jianban-form2.html?roleId=1'
//        this.processInstanceId = processInstanceId
//        url+="&processInstanceId="+this.processInstanceId
//        this.url = url
      },

      methods: {
        getProcessInstanceId() {
          let  processInstanceId = wx.getStorageSync('processInstanceId')
          this.processInstanceId = processInstanceId

          let containerId = wx.getStorageSync('containerId')
          this.containerId = containerId

          wx.removeStorageSync('processInstanceId')

        },
        loadPage() {
          let url = 'http://127.0.0.1/mp-jianban-web/jianban-form2.html?roleId=1'
          //let url = 'https://dmodev.cn/mp-jianban-web/jianban-form2.html?roleId=1'
          if(this.processInstanceId!=''){

            url+="&processInstanceId="+this.processInstanceId

            url+="&containerId="+this.containerId
          }
          this.url = url
        }

      }

    }

</script>


<style>

</style>
