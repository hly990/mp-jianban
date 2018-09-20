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
          taskId: ''
        }
      },
      onShow() {
        this.getTaskId()
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
        getTaskId() {
          if(this.$root.$mp.query && this.$root.$mp.query.taskId) {
            this.taskId = this.$root.$mp.query.taskId
          }
        },
        loadPage() {
          let roleId = wx.getStorageSync('roleId')
          let url = 'http://127.0.0.1/mp-jianban-web/jianban-form2.html?roleId='+roleId
          //let url = 'https://dmodev.cn/mp-jianban-web/jianban-form2.html?roleId='++roleId
          if(this.processInstanceId!=''){

            url+="&taskId="+this.taskId

          }
          this.url = url
        }

      }

    }

</script>


<style>

</style>
