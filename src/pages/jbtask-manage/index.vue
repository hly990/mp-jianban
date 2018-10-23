<template>
  <div class="page">
    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">任务名称</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindinputName" placeholder=" " :value="name"/>
          </div>
        </div>
      </div>
      <span style="font-size: 2vh;font-style: italic">{{alertMessage}}</span>

      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">完成时间</div>
          </div>
          <div class="weui-cell__bd">
            <picker class="weui-btn" mode="date" :value="date" start="2018-08-01" end="2030-09-01" @change="bindDateChange">
              <!--<button type="default">日期选择器</button>-->
              <input class="weui-input" placeholder=" 请选择日期" :value="date" disabled="disabled"/>
            </picker>
          </div>
        </div>
      </div>

      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">微信群提醒给</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" @input="bindcc" placeholder=" " :value="cc"/>
          </div>
        </div>
      </div>
      <span style="font-size: 2vh;font-style: italic">输入被提醒人微信昵称，多人以,分割</span>

    </div>
    <div class="weui-btn-area">
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" @click="saveTask">确定</a>
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
        date: '请选择日期',
        cc: '',
        alertMessage: '',
        save: true,
        taskId: '',
        parentId: '',
        projectId: ''
      }
    },

    onShow() {
      this.getParams()
      this.initData()

    },

    onUnload() {
      console.log("getCurrentPages().length=="+getCurrentPages().length)
    },

    mounted () {
//        if (this.$root.$mp.query && this.$root.$mp.query.from) {
//          this.from = this.$root.$mp.query.from
//        }
    },

    methods: {
      bindinputName (e) {
        if(e.mp.detail.value){
          this.alertMessage = ''
          this.name = e.mp.detail.value
          console.log(e.mp.detail.value)
        }
      },
      bindDateChange(e) {
        console.log('选中的日期为：' + e.mp.detail.value);
        this.date = e.mp.detail.value
      },
      bindcc (e) {
        if(e.mp.detail.value){
          this.cc = e.mp.detail.value
          console.log(e.mp.detail.value)
        }
      },
      saveTask(){
        console.log('this.name=='+this.name)
        if (this.$root.$mp.query && this.$root.$mp.query.taskId) {
          this.taskId = this.$root.$mp.query.taskId
        }
        if (this.$root.$mp.query && this.$root.$mp.query.parentId) {
          this.parentId = this.$root.$mp.query.parentId
        }
        //日期格式check
        var regEx = /^\d{4}-\d{2}-\d{2}$/;
        let isDate = this.date.match(regEx) != null;
        if(!isDate){
          this.date = ''
        }

        console.log("this.taskId="+this.taskId+"\r\n this.parentId="+this.parentId+"\r\n this.date"+this.date)

        if(this.name==''){
          //alert('name is null.')
          this.alertMessage = '任务名称不能为空。'
          this.save = false
        }else{
          this.save = true
        }

        if(this.save){
          if(this.taskId==''){ //insert
            let data = {
              name: this.name,
              expirationTime: this.date,
              cc: this.cc,
              parentId: this.parentId,
              projectId: this.projectId
            }
            api.get('/addTask.do', data).then(response => {
              console.log(JSON.stringify(response))
              this.taskId==''
              this.alertMessage = ''
              this.date = '请选择日期'
              this.cc = ''
              this.name = ''
//              wx.navigateTo({
//                url: '/pages/jbtask/main?parentId='+this.parentId
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
              expirationTime: this.date,
              cc: this.cc,
              exWorkFlowInstanceId: this.taskId
            }
            api.get('/updateTask.do', data).then(response => {
              console.log(JSON.stringify(response))
              this.taskId==''
              this.alertMessage = ''
              this.date = '请选择日期'
              this.cc = ''
              this.name = ''
//              wx.navigateTo({
//                url: '/pages/jbtask/main?parentId='+this.parentId
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

      getParams(){
        if(this.$root.$mp.query && this.$root.$mp.query.parentId){
          console.log("parentId=="+this.$root.$mp.query.parentId)
          this.parentId = this.$root.$mp.query.parentId
        }
        this.projectId = wx.getStorageSync('projectId')
      },

      initData() {
        if(this.$root.$mp.query && this.$root.$mp.query.taskId){
          let data = {
            exWorkFlowInstanceId: this.$root.$mp.query.taskId
          }
          //set init data
          api.get('/getTask.do', data).then(response => {
            let instance = response[1]
            console.log(JSON.stringify(response))

            let returnDate = '请选择日期'

            //format date to yyyy-mm-dd
            console.log("instance.expirationTime=="+instance.expirationTime)
            var expirationTime = instance.expirationTime
            if(instance.expirationTime!=undefined && instance.expirationTime!=null){
              returnDate = instance.expirationTime.substr(0,10)
              var d = new Date(instance.expirationTime),
                month = '' + (d.getMonth() + 1),
                day = '' + d.getDate(),
                year = d.getFullYear();

              if (month.length < 2) month = '0' + month;
              if (day.length < 2) day = '0' + day;

              returnDate = [year, month, day].join('-');

            }

            this.name = instance.name
            this.date = returnDate
            this.cc = instance.cc
            this.alertMessage = ''

          }).catch(error => {
            console.log(error)
          })

        }else {

          this.alertMessage = ''
          this.date = '请选择日期'
          this.cc = ''
          this.name = ''
        }
      },

    }

  }
</script>
