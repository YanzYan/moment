<template>
    <div>
        <scroll-view scroll-y @scrolltoupper="upper()" @scrolltolower="lower()" enable-back-to-top="true">
            <ul class="moments-list">
                <li v-for="(item,idx) in list">
                    <div class="user-info">
                        <img class="avatar" :src="item.user.avatar" />
                        <div>
                            <p class="user-name" v-text="item.user.name"></p>
                            <p class="user-school" v-text="item.user.entity_name"></p>
                        </div>
                    </div>
                    <div class="moments-con">
                        <p class="font" v-if="item.post.holder.content" v-text="item.post.holder.content"></p>
                        <div class="wrap-imgs" v-if="item.post.images.length" :class="{'imgs-1': item.post.images.length==1, 'imgs-4': item.post.images.length==4}">
                            <img class="avatar" v-for="img in item.post.images" :src="img" />
                        </div>
                        <p class="time" v-text="time[idx]"></p>
                    </div>
                    <ul class="like-list" v-if="item.ups.length" >
                        <li v-for="up in item.ups" v-text="up.name"><span class="dot">,</span></li>
                    </ul>
                </li>
            </ul>
        </scroll-view>
    </div>
</template>
<script>

    export default {
        data(){
          return {
            list: [],
            time: [],
            nt: '',
            api: 'https://aggr.anlaiye.com.cn/aggre/user/1311139/follow/feed/list?appid=1&appplt=aph&token=c42832d72cdb08f39d2d4c930327ecf7&appver=3.1.0&pageSize=20'
          }
        },
        methods:{
            timestampToTime(timestamp) {
                var date = new Date(timestamp),//时间戳为10位需*1000，时间戳为13位的话不需乘1000
                Y = date.getFullYear() + '-',
                M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-',
                D = date.getDate() + ' ',
                h = date.getHours() + ':',
                m = date.getMinutes() + ':',
                s = date.getSeconds();
                return Y+M+D+h+m+s;
            },

            getList(){
              var that = this;
              var api = this.api;
              wx.request({
                url: api, 
                header: {
                    'content-type': 'application/json' // 默认值
                },
                success: function(res) {
                    that.list = [], that.time = [], that.nt = '';
                    that.nt = res.data.data.nt;
                    that.list = res.data.data.list;
                    for(var i=0;i<that.list.length;i++){
                        var time = that.timestampToTime(that.list[i].display_time);
                        that.time.push(time);
                    }
                    console.log(that.time);
                }
              })
            },
            // 下拉刷新
            upper () {
              this.getList();
            },
            // 上拉加载
            lower(){
              var that = this;
              var api = this.api + '&nt='+this.nt;
              wx.request({
                url: api, 
                header: {
                    'content-type': 'application/json' // 默认值
                },
                success: function(res) {
                    that.nt = res.data.data.nt;
                    that.list = that.list.concat(res.data.data.list);
                    for(var i=that.list.length-20;i<that.list.length;i++){
                        var time = that.timestampToTime(that.list[i].display_time);
                        that.time.push(time);
                    }
                    console.log(that.time);
                }
              })
            }  
        },
        created(){
            this.getList();
        }
    }
</script>
<style lang="scss" scoped>
scroll-view{height: 1142rpx;}
.moments-list{
    >li{
        padding: 40rpx;
        border-bottom: 1px solid #eee;
        .user-info{
            display: flex;
            align-items: center;
            .avatar{
                width:110rpx;
                height:110rpx;
                border-radius:50%;
                margin-right: 30rpx;
            }
            .user-name{
                font-size: 32rpx;
                color: #222;
                margin-bottom: 10rpx;
            }
            .user-school{
                font-size: 22rpx;
                color: #999;
            }
        }
        .moments-con{
            margin-top: 40rpx;
            .font{
                font-size: 28rpx;
                color: #222;
                margin-bottom: 20rpx;
            }
            .time{
                font-size: 26rpx;
                color: #666;
                margin: 10rpx 0 26rpx;
            }
            .wrap-imgs{
                img{
                    width: 210rpx;
                    height: 210rpx;
                    margin-right: 20rpx;
                    &:nth-child(3n){
                        margin-right: 0;
                    }
                }
                
                &.imgs-1{
                    img{
                        width: 320rpx;
                        height: 320rpx;
                    }
                }
                &.imgs-4{
                    img{
                        width: 210rpx;
                        height: 210rpx;
                        &:nth-child(2n){
                            margin-right: 230rpx;
                        }
                        &:nth-child(3n){
                            margin-right: 20rpx;
                        }
                    }
                    
                }
            }
        }
        .like-list{
            background: #f3f3f5 url(../../static/images/like.png) no-repeat 20rpx center;
            font-size: 24rpx;
            color: pink;
            padding: 20rpx 20rpx 20rpx 100rpx;
            li{
                display: inline-block;
                .dot{
                    color: #222;
                    padding-right:14rpx;
                }
                &:last-child{
                    .dot{
                        display: none;
                    }
                }
            }
        }
    }
    
}
</style>


