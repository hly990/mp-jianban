<template>
  <div class="page">
    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">登录名</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindUserName" :value="username"  @focus="clearPlaceHolder" :placeholder="tips"/>
          </div>
        </div>
      </div>
      <span>{{alertMessage1}}</span>

      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">密码</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindPassword" :value="password" placeholder=" "/>
          </div>
        </div>
      </div>
      <span>{{alertMessage2}}</span>

      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">真实姓名</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindDes" :value="des" placeholder=" "/>
          </div>
        </div>
      </div>
      <span>{{alertMessage3}}</span>

    </div>

    <div class="weui-btn-area">
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" @click="saveUser">确定</a>
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
        username: '',
        password: '',
        des: '',
        tips: '推荐使用手机号',
        alertMessage1: '',
        alertMessage2: '',
        alertMessage3: '',
        save: true,
        projectId:'',
        exUserId: ''
      }
    },

    onShow() {
      this.initData()
      this.getUser()
    },

    methods: {
      bindUserName (e) {
        this.username = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      bindPassword (e) {
        this.password = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      bindDes (e) {
        this.des = e.mp.detail.value
        console.log(e.mp.detail.value)
      },
      clearPlaceHolder(e) {
        this.tips = ''
      },
      saveUser(){
        if (this.$root.$mp.query && this.$root.$mp.query.exUserId) {
          this.exUserId = this.$root.$mp.query.exUserId
        }
        console.log("this.projectId="+this.projectId)

        if(this.username==''){
          //alert('name is null.')
          this.alertMessage1 = '登录名不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.password==''){
          //alert('name is null.')
          this.alertMessage2 = '密码不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.des==''){
          this.alertMessage3 = '真实姓名不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.save){
          if(this.exUserId==''){ //insert
            let data = {
              username: this.username,
              password: this.password,
              des: this.des,
              projectId: wx.getStorageSync('projectId')
            }
            api.get('/addUser.do', data).then(response => {
              console.log(JSON.stringify(response))
              wx.navigateBack({
                delta: 1
              })

            }).catch(error => {
              console.log(error)
            })
          }else{ //update
            let data = {
              username: this.username,
              password: this.password,
              des: this.des,
              exUserId: this.exUserId
            }
            api.get('/updateUser.do', data).then(response => {
              console.log(JSON.stringify(response))
              wx.navigateBack({
                delta: 1
              })


            }).catch(error => {
              console.log(error)
            })

          }

        }

      },
      initData() {
        this.alertMessage1 = ''
        this.alertMessage2 = ''
        this.alertMessage3 = ''
        this.username = ''
        this.password = ''
        this.des = ''
      },

      getUser() {
        if(this.$root.$mp.query && this.$root.$mp.query.userId){
          let data = {
            exUserId: this.$root.$mp.query.userId
          }
          //get the User
          api.get('/getUser.do', data).then(response => {
            let instance = response[1]
            console.log("/getUser.do :"+JSON.stringify(response))

            this.username = instance.username
            this.password = instance.password
            this.des = instance.des
            this.exUserId = instance.exUserId

          }).catch(error => {
            console.log(error)
          })

        }
      },

      mounted () {

      }
    }
  }
</script>
