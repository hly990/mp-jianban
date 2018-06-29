<template>
  <div class="page">
    <!--<div class="page__hd">-->
    <!--<div class="page__title">Input</div>-->
    <!--<div class="page__desc">表单输入</div>-->
    <!--</div>-->

    <div class="pt-2 page__bd">
      <div class="weui-toptips weui-toptips_warn" v-if="showTopTips">错误提示</div>

      <div class="weui-cells__title">表单</div>
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">流程名称</div>
          </div>
          <div class="weui-cell__bd">
            <input class="weui-input" placeholder="XXX" />
          </div>
        </div>
        <div class="weui-cell weui-cell_input weui-cell_vcode">
          <div class="weui-cell__hd">
            <div class="weui-label">上传资料</div>
          </div>
          <div class="weui-uploader__bd">
            <div class="weui-uploader__files" id="uploaderFiles">
              <block v-for="item in files" :key="index">
                <div class="weui-uploader__file" @click="predivImage" :id="item">
                  <image class="weui-uploader__img" :src="item" mode="aspectFill" />
                </div>
              </block>
            </div>
            <div class="weui-uploader__input-box">
              <div class="weui-uploader__input" @click="chooseImage"></div>
            </div>
          </div>
        </div>

        <div class="weui-cell weui-cell_input">
          <div class="weui-cell__hd">
            <div class="weui-label">到期时间</div>
          </div>
          <div class="weui-cell__bd">
            <picker mode="date" value="date" start="2015-09-01" end="2017-09-01" @change="bindDateChange">
              <div class="weui-input">{{date}}</div>
            </picker>
          </div>
        </div>
      </div>

      <div class="weui-cells__title">备注</div>
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell">
          <div class="weui-cell__bd">
            <textarea class="" placeholder="请输入文本" style="height: 3.3em" />
            <div class="weui-textarea-counter">0/200</div>
          </div>
        </div>
      </div>

      <div class="weui-cells__title">状态</div>
      <div class="weui-cells weui-cells_after-title">
        <div class="weui-cell weui-cell_select">
          <div class="weui-cell__bd">
            <picker @change="bindAccountChange" :range="accounts">
              <div class="weui-select">{{accounts[accountsIndex]}}</div>
            </picker>
          </div>
        </div>

      </div>

      <div class="weui-btn-area">
        <button class="weui-btn" type="default" @click="showTopTipsFun">确定</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        files: [],
        showTopTips: false,
        time: '09:01',
        date: '2015-09-01',
        countryCodes: ["+86", "+80", "+84", "+87"],
        countryCodesIndex: 0,
        countries: ["中国", "美国", "英国"],
        countryIndex: 0,
        accounts: ["未开始", "进行中", "已完成", "已过期"],
        accountsIndex: 0,

        radioItems: [
          { name: 'cell standard', value: '0' },
          { name: 'cell standard', value: '1', checked: true }
        ],
        checkboxItems: [
          { name: 'standard is dealt for u.', value: '0', checked: true },
          { name: 'standard is dealicient for u.', value: '1', checked: false }
        ],

        isAgree: false
      }
    },
    methods: {
      checkboxChange(e) {
        console.log('checkbox发生change事件，携带value值为：' + e.mp.detail.value);
        var checkboxItems = this.checkboxItems, values = e.mp.detail.value;
        for (var i = 0, lenI = checkboxItems.length; i < lenI; ++i) {
          checkboxItems[i].checked = false;

          for (var j = 0, lenJ = values.length; j < lenJ; ++j) {
            if (checkboxItems[i].value == values[j]) {
              checkboxItems[i].checked = true;
              break;
            }
          }
        }
        this.checkboxItems = checkboxItems;
      },
      radioChange(e) {
        console.log('radio发生change事件，携带value值为：' + e.mp.detail.value);
        let radioItems = this.radioItems;
        for (let i = 0; i < radioItems.length; ++i) {
          radioItems[i].checked = radioItems[i].value === e.mp.detail.value;
        }
        this.radioItems = radioItems;
      },
      switchChange(e) {
        console.log("switch发生change事件，携带value值为："+ e.mp.detail.value);
      },
      bindDateChange(e) {
        this.date = e.mp.detail.value;
        console.log(e.mp.detail.value);
      },
      bindTimeChange(e) {
        this.time = e.mp.detail.value;
        console.log(e.mp.detail.value);
      },
      showTopTipsFun() {
        this.showTopTips = true;
        setTimeout(() => {
          this.showTopTips = false;
        }, 2000)
      },

      bindCountryChange(e) {
        this.countryIndex = e.mp.detail.value;
      },
      bindAccountChange(e) {
        this.accountsIndex = e.mp.detail.value;
      },
      bindCountryCodeChange(e) {
        this.countryCodesIndex = e.mp.detail.value;
      },
      bindAgreeChange(e) {
        this.isAgree = !this.isAgree;
      },
      chooseImage(e) {
        var _this = this;
        wx.chooseImage({
          sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
          sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
          success: function (res) {
            // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
            _this.files = _this.files.concat(res.tempFilePaths)
          },
          fail: function () {
            console.log('fail');
          },
          complete: function () {
            console.log('commplete');
          }
        })
      },
      predivImage(e) {
        console.log(e);
        wx.previewImage({
          current: e.currentTarget.id, // 当前显示图片的http链接
          urls: this.files // 需要预览的图片http链接列表
        })
      }
    }
  }
</script>

<style>

</style>
