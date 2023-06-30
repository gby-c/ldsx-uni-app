<template>
  <view class="container">
    <u-popup :show="loginFlag"
             mode="center"
             :round="10"
             custom-style="width: 80%; height: 40%; display: flex; justify-content: center; align-items: center;">
      <u--form :model="loginInfo">
        <u-form-item>
          <template>
            <span style="text-align: center">登录后更精彩</span>
          </template>
        </u-form-item>
        <u-form-item label="账号" prop="userName">
          <u--input v-model="loginInfo.userName" placeholder="请填写账号"></u--input>
        </u-form-item>
        <u-form-item label="密码">
          <u--input v-model="loginInfo.password" placeholder="请填写密码" type="password"></u--input>
        </u-form-item>
        <u-form-item>
          <u-button customStyle="width: 200rpx" type="primary" @click="userQuery">登录</u-button>
        </u-form-item>
      </u--form>
    </u-popup>
    <view class="userCover">
      <u-image :src="user.userCover? user.userCover:'https://buf.gbyttt.cn/img/202306302123217.JPG'"
               width="100%"></u-image>
    </view>
    <view class="userPic">
      <u-image :src="user.userPic? user.userPic : '../../static/loginNo.png'" shape="circle" width="150px"
               height="150px" custom-style="margin-top: -75px; margin-left: 10px"></u-image>
      <u-image :src="user.userPic? '../../static/loginYes.png' : '../../static/loginNo.png'" shape="circle" width="30px"
               height="30px" custom-style="margin-top: -35px; margin-left: 120px"></u-image>
    </view>
    <view class="selfInfo">
      <view class="userName">
        <u--text type="primary" :text="user.userName?user.userName:'请登录'" align="center" size="25"></u--text>
      </view>
      <view class="userContent" style="width: 160px">
        <u--text type="info" :text="user.content && user.content !== ''? user.content : '快跟大家介绍一下你吧'"
                 :block="true"></u--text>
      </view>
      <view class="update" style="display: flex; justify-content: center;">
        <u-button icon="edit-pen" custom-style="width: 40px" @click="()=>{
          uni.navigateTo({
	          url:'/pages/editInfo/editInfo'
          });
        }"></u-button>
      </view>
    </view>
    <view class="selfInfo" style="width: 150px; justify-content: center">
      <view class="userSex" style="margin-right: 20px">
        <u--text :prefixIcon="user.sex==='男'?'man':'woman'" iconStyle="font-size: 19px" :text="user.sex"></u--text>
      </view>
      <view class="userAge">
        <u--text prefixIcon="account-fill" iconStyle="font-size: 19px" :text="user.age"></u--text>
      </view>
    </view>
  </view>
</template>

<script>
import route from "../../uni_modules/uview-ui/libs/util/route";

export default {
  data() {
    return {
      user: {},
      loginInfo: {
        userName: '',
        password: ''
      },
      loginFlag: false,
      rules: {
        'loginInfo.userName': {
          require: true,
          message: '请填写用户名'
        },
        'loginInfo.password': {
          require: true,
          message: '请填写密码'
        }
      }
    }
  },
  onLoad() {
    let userStr = localStorage.getItem('userInfo')
    if (userStr) {
      this.user = JSON.parse(userStr)
      console.log(this.user)
    } else {
      this.loginFlag = true;
    }
  },
  methods: {
    userQuery() {
      if (this.loginInfo.userName === '') {
        uni.$u.toast("请输入账号")
      } else if (this.loginInfo.password === '') {
        uni.$u.toast("请输入密码")
      } else {
        uni.request({
          url: this.path + '/user/login',
          method: 'GET',
          data: this.loginInfo,
          success: (res) => {
            if (res.data.status === 200) {
              uni.$u.toast("登陆成功")
              this.loginFlag = false
              this.user = res.data.data
              localStorage.setItem('userInfo', JSON.stringify(this.user))
            }
          },
          fail: (res) => {
            console.log(res.data)
          }
        })
      }
    }
  }
}
</script>

<style scoped lang="scss">
.userName {
  width: 150px;
  height: 60px;
  display: flex;
  align-items: center;
}

.selfInfo {
  display: flex;
  margin-left: 10px;
}
</style>