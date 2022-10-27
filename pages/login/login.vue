<template>
	<view style="margin: 40% 0 60% 0;">
		<u-notify ref="uNotify" message="Hi uView"></u-notify>
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;">
			<u-avatar :src="avatarUrl" size="250rpx"></u-avatar>
		</view>
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 30%;">为能提供更优质的服务，请先登录！
			{{info.nickName}}
		</view>
		<view style="margin: 0% 20% 0% 20%;display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;">
			<u-button type="primary" :plain="true" text="登录" @click="login"></u-button>

		</view>
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 30%;">
			<view>
				<u--text text="暂不登录" decoration="underline" color="#0055ff" @click="esc"></u--text>
			</view>
		</view>

	</view>

</template>

<script>
	import Qs from 'qs'
	export default {
		data() {
			return {
				info: {
					nickName: '',
					avatarUrl: '',
					language: '',

				},
				userid: uni.getStorageSync('uid'),
				userinfo: {
					openid: '',
					session_key: '',
				}

			}
		},
		onLoad() {
			let that = this
			wx.login({
				success: res => {
					console.log('code:' + res.code); // 先login得到code
					if (res.code) {
						var url = 'http://127.0.0.1:8000/getopenid/'
						wx.request({
							url: url,
							data: {
								'code': res.code
							},
							method: 'GET',
							success: function(res) {
								// res.data.openid 即为所求openid
								that.userinfo.openid = res.data;
								console.log('openid:' + res.data);
								console.log('获取成功')
								uni.setStorage({
									key: 'uid', //存储到本地的名字
									data: res.data.openid, //存储的数据
									success() {
										console.log('存储uid')
									}
								})
							}
						});
					}
				}
			});

		},
		methods: {
			esc() {
				uni.switchTab({
					url: '../activity/activity'
				})

			},
			login() {
				let that = this
				uni.getUserProfile({
					desc: "用于完善用户信息",
					success: (res) => {
						that.info = res.userInfo;
						console.log(res)
						uni.showToast({
							icon: "none",
							title: '登陆成功，即将跳转'
						})
						uni.request({
							url: 'http://127.0.0.1:8000/login/',
							header: {
								'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
							},
							method: 'POST',
							data: {
								openid: that.userinfo.openid,
								username: that.info.nickName,
								headimg: that.info.avatarUrl,
							},
							dataType: 'json',
							success: res => {
								console.info("success");
								console.log(res);
								uni.setStorage({
									key: 'name', //存储到本地的名字
									data: this.info.nickName, //存储的数据
									success() {
										console.log('存储成功')
									}
								})
								uni.setStorage({
									key: 'img', //存储到本地的名字
									data: this.info.avatarUrl, //存储的数据
									success() {
										console.log('存储成功2')
									}
								})
								uni.setStorage({
									key: 'key', //存储到本地的名字
									data: 520, //存储的数据
									success() {
										console.log('存储成功la')
									}
								})
								uni.switchTab({
									url: '../person/person'
								})
							},
							fail: () => {},
							complete: () => {}
						})
					},
					fail: (err) => {

					}
				})
			},

		},
	}
</script>

<style>

</style>
