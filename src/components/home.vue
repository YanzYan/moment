<template>
  <div class="container">
    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" /> 
    </div>
    <!-- <button @click="getAuth()" open-type='getUserInfo'>获取用户信息</button> -->
    
    <swiper indicator-dots="true" autoplay="true" interval="5000" duration="1000">
      <swiper-item  v-for="(item, index) in imgUrls">
        <image :src="item" class="slide-image" width="355" height="150"/>
      </swiper-item>
    </swiper> 
    <!-- <a href="/pages/counter/main" class="counter">去往Vuex示例页面</a> -->
    <!-- <bottom isHomeCur='true'></bottom> -->
  </div>
</template>

 <script>
    export default{
        data () {
            return {
            userInfo: {},
            imgUrls: [
                'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
                'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
                'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
            ]
            }
        },

        methods: {
            bindViewTap () {
                const url = '../logs/main'
                wx.navigateTo({ url })
            },
            getAuth(){
              wx.getSetting({
                success(res) {
                    if (!res.authSetting['scope.record']) {
                        wx.authorize({
                            scope: 'scope.record',
                            success:() => {
                                // 用户已经同意小程序使用录音功能，后续调用 wx.startRecord 接口不会弹窗询问
                                wx.startRecord({
                                  success:() => {
                                    this.getUserInfo();
                                  }
                                });
                            }
                        })
                    }
                }
              })
            },
            getUserInfo () {
            // 调用登录接口
              wx.login({
                  success: () => {
                    wx.getUserInfo({
                        success: (res) => {
                          this.userInfo = res.userInfo;
                          console.log(this.userInfo)
                        },
                        fail: (err)=>{
                          console.log(err)
                        }
                        
                    })
                  }
              })
            }
        },

        created () {
            // 调用应用实例的方法获取全局数据
            this.getUserInfo()
        }

    }
</script>

<style scoped lang="scss">
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 40rpx;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
  border-radius: 24rpx;
}

swiper{
  width: 100%;
  height: 480rpx;

  swiper-item{
    text-align:center;
  }
}

</style>


