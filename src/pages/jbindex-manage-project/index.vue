<template>
  <div class="page">
    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">项目名称</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindinputName" placeholder=" "/>
          </div>
        </div>
      </div>
      <span>{{alertMessage}}</span>
    </div>
    <div class="weui-btn-area">
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" @click="saveProject">确定</a>
    </div>

  </div>
</template>

<script>
  // Use Vuex
  import store from './store'
  import api from '@/api/index'

  export default {
    data() {
      return {
        name: '',
        alertMessage:'',
        save: true,
        projectId:''
      }
    },

    methods: {
      bindinputName (e) {
        this.name = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      saveProject(){
        console.log('this.name=='+this.name)
        if (this.$root.$mp.query && this.$root.$mp.query.projectId) {
          this.projectId = this.$root.$mp.query.projectId
        }
        console.log("this.projectId="+this.projectId+"\r\n wx.getStorageSync('userId')=="+wx.getStorageSync('userId'))

        if(this.name==''){
          //alert('name is null.')
          this.alertMessage = '项目名称不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.save){
          if(this.projectId==''){ //insert
            let data = {
              name: this.name,
              userId: wx.getStorageSync('userId'),
              exWorkFlowTemplateId: 1
            }
            api.get('/addProject.do', data).then(response => {
              console.log(JSON.stringify(response))
              this.alertMessage = ''
              this.name = ''
//              wx.navigateTo({
//                url: '/pages/jbindex/main'
//              })
              wx.navigateBack({
                delta: 1
              })

            }).catch(error => {
              console.log(error)
            })
          }else{ //update
            let data = {
              name: this.name,
              projectId: this.projectId
            }
            api.get('/updateProject.do', data).then(response => {
              console.log(JSON.stringify(response))
              this.alertMessage = ''
              this.name = ''
//              wx.navigateTo({
//                url: '/pages/jbindex/main'
//              })
              wx.navigateBack({
                delta: 1
              })


            }).catch(error => {
              console.log(error)
            })

          }

        }

      },
      mounted () {
//        if (this.$root.$mp.query && this.$root.$mp.query.from) {
//          this.from = this.$root.$mp.query.from
//        }
      }
    }
  }
</script>
