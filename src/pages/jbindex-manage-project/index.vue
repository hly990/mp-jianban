<template>
  <div class="page">
    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">项目名称</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindinputName" :value="name" placeholder=" "/>
          </div>
        </div>
      </div>
      <span>{{alertMessage}}</span>
    </div>

    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">项目微信群</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindinputWechatGroup" :value="wechatGroup" placeholder=" "/>
          </div>
        </div>
      </div>
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
        wechatGroup: '',
        alertMessage:'',
        save: true,
        projectId:''
      }
    },

    onShow(){
      this.initData()
      this.getProject()
    },

    methods: {
      bindinputName (e) {
        this.name = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      bindinputWechatGroup (e) {
        this.wechatGroup = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      saveProject(){
        if (this.$root.$mp.query && this.$root.$mp.query.projectId) {
          this.projectId = this.$root.$mp.query.projectId
        }
        console.log("this.projectId="+this.projectId+"\r\n wx.getStorageSync('userId')=="+wx.getStorageSync('userId'))

        if(this.name==''){
          this.alertMessage = '项目名称不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.save){
          if(this.projectId==''){ //insert
            let data = {
              name: this.name,
              wechatGroup:this.wechatGroup,
              userId: wx.getStorageSync('userId'),
              exWorkFlowTemplateId: 1
            }
            api.get('/addProject.do', data).then(response => {
              this.alertMessage = ''
              this.name = ''
              wx.navigateBack({
                delta: 1
              })

            }).catch(error => {
              console.log(error)
            })
          }else{ //update
            let data = {
              name: this.name,
              wechatGroup:this.wechatGroup,
              projectId: this.projectId
            }
            api.get('/updateProject.do', data).then(response => {
              console.log(JSON.stringify(response))
              this.alertMessage = ''
              this.name = ''
              wx.navigateBack({
                delta: 1
              })


            }).catch(error => {
              console.log(error)
            })

          }

        }//if end


      },

      initData() {
        this.alertMessage = ''
        this.name = ''
        this.wechatGroup = ''
      },

      getProject() {
        if(this.$root.$mp.query && this.$root.$mp.query.projectId){
          let data = {
            exProjectId: this.$root.$mp.query.projectId
          }
          //get the User
          api.get('/getProject.do', data).then(response => {
            let project = response[1]

            this.name = project.name
            this.wechatGroup = project.wechatGroup

          }).catch(error => {
            console.log(error)
          })

        }
      }

    }
  }
</script>
