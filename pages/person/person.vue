<template>
	<view>
		<view class="content-item">
		<view style="width:100%;height:120px;background-color: #ffffff;">
			
		</view>
		<view class="user-info">
		<view>
		<u-avatar size="60" :src="avatarUrl" ></u-avatar>
		</view>
		<view class="name">
		<view>
			
		<view style="padding-left: 20px;">
		<u-cell-group :border="false">
				<u-cell :border="false" :title="`${nickName}`":label="`个人主页>`" @click="grzy"></u-cell>
				
			</u-cell-group>
		</view></view>
		</view>
		</view>
		</view>
		<u-cell-group>
			<u-cell
			    title="我的投票"
			    isLink
			    url="/pages/myrelease/myrelease"
			></u-cell>
			
		</u-cell-group>
		<!-- <u-cell-group>
			<u-cell
			    title="设置"
			    isLink
			    url="/pages/configure/configure"
			></u-cell>
			
		</u-cell-group> -->
		<view style="background-color: #bfbfbf;margin-top: 5%;"><u-button type="primary" :plain="true" :hairline="true" color="#bfbfbf" text="退出登录" @click="q"></u-button></view>
	</view>
</template>

<script>
	export default {
		data() {
			return { 
				
					nickName:uni.getStorageSync('name'),
					avatarUrl:uni.getStorageSync('img'),
					userid:uni.getStorageSync('uid'),
					language:'',
                    key:uni.getStorageSync('key'),
				cover: 'https://cdn.uviewui.com/uview/album/1.jpg',
			}
		},
		onLoad() {
			this.key = uni.getStorageSync('key');
			this.avatarUrl = uni.getStorageSync('img');
					 if(this.key == 520){   //判断是否有记录
					    	
					    }else {
					        uni.redirectTo({
					            url: '../login/login'
					        }) 
						}
				},
		 methods: {
			 q(){
			 	  uni.clearStorage()
			      uni.redirectTo({
			          url: '../login/login'
			      }) 
			 },	
			 grzy(){
				 let id = this.userid
			 	 uni.navigateTo({
			 	     url: '../homepage/homepage'
			 	 })
			     
			 },	
			
		}

	}
</script>

<style>
	
.container{
width: 100%;
}
.profile{
width: 100%;
}
.user-info{
position: absolute;
top: 30px;
left: 30px;
display: flex;
align-items: center;

}
.user-info .name{
display: flex;
}
.content-item{
display: flex;

flex-direction: column;
border-radius: 4px;
background-color: #FFFFFF;
justify-content: space-around;
}
.content-item .title{
padding: 15px;
color: #5A5B5C;
font-size: 16px;
font-weight: bold;
}
.content-show{
display: flex;
flex-direction: row;
justify-content: space-around;
align-items: center;
}
.money-item{
display: flex;
flex-direction: column;
align-items: center;
padding: 20px;
}

.service-item{
display: flex;
flex-direction: column;
align-items: center;
padding: 20px;
}
.service-item .service-text{
font-size: 12px;
color: #919293;
}
.order-tab{
display: flex;
flex-direction: column;
align-items: center;
padding: 15px;
}
.order-title{
font-size: 14px;
padding-top: 10px;
color: #919293;
padding-bottom: 8px;
}
.bg-ware {
  position: absolute;
  left: 0;
  bottom: -2rpx;
  width: 100%;
  mix-blend-mode: screen;
  height: 120rpx;
}
</style>
