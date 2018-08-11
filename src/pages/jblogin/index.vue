<template>
  <div class="page">
    <!-- 用户授权 -->
    <div class="text-center border-bottom mb-3 py-5 border-0">
      <image src="/static/images/svg/jianban.svg" style="width: 423rpx; height: 100rpx"/>
    </div>

    <!--<p class="text-left mb-3">该程序将获取以下授权:</p>-->
    <!--<p class="small text-left ml-4 mb-2">获得你的公开信息(昵称, 头像等)</p>-->
    <!--<p class="small text-left ml-4 mb-5">扫码登录cnode账号</p>-->

    <div class="p-2 page__bd">
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">用户名</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" placeholder=" "/>
          </div>
        </div>
      </div>

      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">密码</div>
          </div>
          <div class="weui-cell__bd">
            <input type="password" class="weui-input" placeholder=" "/>
          </div>
        </div>
      </div>
    </div>
    <div class="weui-btn-area">
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" @click="login">登录</a>
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbindex/main'">登录</a>
    </div>
    <!--<a class="btn btn-lg text-white btn-block btn-secondary px-0"  @click="refuse">取消</a>-->

  </div>
</template>

<script>
  // Use Vuex
  import store from './store'
  import api from '@/api/index'

  export default {
    methods: {
      refuse () {
        console.log('refuse')
        wx.navigateBack({delta: 1})
      },
      login() {
        let data = {
          username: "krisv",
          password: "krisv"
        }
        // 获取项目列表接口
        api.get('/auth.do', data).then(response => {
          console.log(JSON.stringify(response))
          console.log(response[1])
          var user = response[1]
          wx.setStorageSync('username', user.username)
          wx.setStorageSync('loginname', user.password)
          wx.setStorageSync('roleId', user.roleId)
          wx.navigateTo({
            url: '/pages/jbindex/main'
          })
        }).catch(error => {
          console.log(error)
        })
      },
      mounted () {
        if (this.$root.$mp.query && this.$root.$mp.query.from) {
          this.from = this.$root.$mp.query.from
        }
      }
    }
  }
</script>
