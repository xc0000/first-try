<template>
	<view class="u-page" style="margin: 2% 5% 2% 5%;">
		<view class="u-demo-block">


			<view class="u-demo-block__content">
				<view class="album">
					<view class="">
						<u-avatar :src="info.headimg"></u-avatar>
					</view>
					<view class="album__content">
						<view style="margin-bottom: -10rpx;"> <text style="font-size: 35rpx;">{{info.username}}</text>
						</view>
						<text style="font-size: 25rpx;color: #b5b5b5;">
							{{info.releasedate}}
						</text>

					</view>
					<view class="album__content">
						<view style="float: right;text-align: right;"> <text
								style="font-size: 25rpx;text-align: right;">第</text><text
								style="font-size: 40rpx;color: #0055ff;text-align: right;">
								<!-- {{info.pspm}} -->test排名
							</text><text style="font-size: 25rpx;text-align: right;">名</text>
							<br> <text style="font-size: 30rpx;font-weight: 600; color: #0055ff;">
								{{info.id}}
							</text><text style="font-size: 25rpx;text-align: right;">编号</text><text
								style="font-size: 30rpx;font-weight: 600; color: #0055ff;text-align: right;">
								{{info.releasevote}}
							</text><text style="font-size: 25rpx;text-align: right;">票</text>
						</view>

					</view>
				</view>
			</view>


			<view style="font-size: 15px;padding-top: 1px; padding-left:1px;color: #ffaa00; " @click="hd()">
				#{{info.activityname}}</view>
			{{info.releasecontent}}
			<u-album :urls="info['releasepicture']" rowCount="4" multipleSize="150rpx" singleSize="700rpx" maxCount="8">
			</u-album>
			<br>
			<view class="u-demo-block__content" style="padding-top: 15rpx;padding-bottom: 5rpx;">
				<u-row justify="space-between" customStyle="margin:5px">
					<u-col span="9">
						<view>
							<u--textarea :disableDefaultPadding="true" height="20" v-model="zzpl" placeholder="输入评论内容"
								autoHeight></u--textarea>
						</view>
					</u-col>
					<u-col span="1">

					</u-col>
					<u-col span="2">
						<u-button type="primary" size=" mini" text="评论" @click="plq()"></u-button>
					</u-col>

					<!-- <u-icon name="eye" color="#2979ff" size="20" label="1"  ></u-icon>
			        </u-col>
					<u-col span="1" > -->


				</u-row>
			</view>
		</view>
		<u-divider></u-divider>


		<!-- 用户评论 -->
		<view>
			<view style="padding-bottom: 5%;"> <text style="font-size: 20px;font-weight: 800;">用户评论</text></view>
		</view>

		<view class="u-demo-block__content" style="padding-right: 1%;">
			<view class="album">
				<view class="">
					<!-- <u-avatar :src="plimg"></u-avatar> -->
				</view>
				<view class="album__content">
					<view class="album__content">
						<view style="margin-bottom: -10rpx;"> <text :bold="true"
								style="font-size: 35rpx;">{{plinfo.username1}}</text></view>
						<view><text style="font-size: 25rpx;color: #b5b5b5;">{{info.time}}</text></view>
						<view class="tex"><text>{{plinfo.plnr}}</text></view>
					</view>
				</view>
			</view>
		</view>

		<u-tabbar :value="value6" @change="name => value6 = name" :fixed="true" :border="false" :placeholder="true"
			:safeAreaInsetBottom="true">
			<view style="width: 100%;padding: 0 5% 0 5%;">
				<u-row>
					<u-col span="10">
						<u-button type="primary" text="立即投票" @click="vote()"></u-button>
					</u-col>


					<u-col span="2">
						<view style="padding-left: 20%;">
							<u-icon name="share-square" size="70rpx" @click="btnClick()"></u-icon>
						</view>
					</u-col>
				</u-row>
			</view>
		</u-tabbar>
		<u-popup :show="show" :round="10" mode="bottom" @close="close" @open="open">
			<view
				style="height: 300rpx;display: flex;flex-direction: column;justify-content: center;align-items: center;">
				<u-row gutter="15">
					<u-col span="6">
						<u-button type="primary" text="分享给朋友" open-type="share"></u-button>
					</u-col>
					<u-col span="6">
						<u-button type="primary" text="分享到朋友圈" @click="AA"></u-button>
					</u-col>
				</u-row>
			</view>
		</u-popup>
	</view>

	</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				userinfo: {
					openid: ''
				},
				pluser: uni.getStorageSync('name'),
				plimg: uni.getStorageSync('img'),
				show: false,
				plsj: '刚刚',
				pl: '',
				zzpl: '',
				id: '',
				isShow: false,
				photo: '',
				info: [

				],
				plinfo: [

				],
			}
		},
		onShareAppMessage(res) {
			// 此处的distSource为分享者的部分信息，需要传递给其他人
			if (distSource) {
				return {
					hdnr: '欢迎使用开发版本',
					hdmc: 0,
					path: '/pages/index/index?id=' + distSource,
					summary: "",
					imageUrl: "https://img.js.design/assets/img/63146ed006e43e3200584b1a.jpg#1e9318d18f7da028f0943d1d5ccd1328"
				}
			}
		},
		//分享到朋友圈
		onShareTimeline(res) {
			let distSource = uni.getStorageSync('distSource');
			if (distSource) {
				return {
					hdnr: '欢迎使用开发版本',
					hdmc: 0,
					query: 'id=' + distSource,
					summary: "",
					imageUrl: "https://img.js.design/assets/img/63146ed006e43e3200584b1a.jpg#1e9318d18f7da028f0943d1d5ccd1328"
				}
			}
		},

		onLoad: function(e) {
			console.log(e.id)
			this.id = e.id;
			let that = this
			wx.login({
				success: res => {
					console.log('code:' + res.code); // 先login得到code
					if (res.code) {
						var url = 'https://lin-undertake.cn/wxapi/getopenid/'
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
			uni.request({
				url: 'http://127.0.0.1:8000/postreleaseiddata/',
				header: {
					'content-type': 'application/x-www-form-urlencoded;charset = utf-8',
				},
				method: 'POST',
				data: {
					releaseid: this.id,
				},
				dataType: 'json',
				success: res => {
					console.log('success')
					// console.log(res.data);
					this.info = res.data
					let i = 0;
					let w = 0;
					var q = '';
					let imglist = []
					for (i = 0; i < this.info['releasepicture'].length; i++) {
						// console.log(this.info['releasepicture'][i]);
						if (this.info['releasepicture'][i] == '"' && w == 1) {
							console.log(q);
							imglist.push('http://127.0.0.1:8000/upload/' + q)
							w = 0;
							q = '';
							continue;
						}
						if (this.info['releasepicture'][i] == '"' && w == 0) {
							w = 1;
							continue;
						}
						if (this.info['releasepicture'][i] != '"' && w == 1) {
							q = q + this.info['releasepicture'][i];
							continue;
						}
					}
					this.info['releasepicture'] = imglist
					console.log(
						this.info['releasepicture'])
					// this.plinfo = res.data.pldata[0];
				},
				fail: () => {},
				complete: () => {}
			})
		},
		methods: {
			hd() {
				uni.navigateTo({
					url: `../gfhd/gfhd`
				})

			},
			vote() {
				let that = this
				uni.request({
					url: 'http://127.0.0.1:8000/postvote/',
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset = utf-8',
					},
					method: 'POST',

					data: {
						openid: that.userinfo.openid,
						username: that.info.username,
						releaseid: that.info.releaseid,
						activityid: that.info.activityid,


					},
					dataType: 'json',
					success: res => {
						console.log('success')
						console.log('这是userinfo的数据', that.userinfo);
						console.log('这是info的数据', that.info);
						if (res.data == "failed") {
							uni.navigateTo({
								url: '../vote_fail/vote_fail'
							})
						} else {
							uni.navigateTo({
								url: '../vote_success/vote_success'
							})
						}

					},
					fail: () => {

					},
					complete: () => {

					}
				})
			},

			btnClick() {
				this.show = true;
			},
			close() {
				this.show = false
			},
			// plq() {
			// 	his.isShow =!this.isShow;
			// },
			plq() {
				let that = this
				uni.request({
					url: 'https://lin-undertake.cn/wxapi/FaBiaoPingLun/',
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset = utf-8',
					},
					method: 'POST',

					data: {
						userid: that.userinfo.openid,
						id: that.id,
						// img:'12121212',
						// hdnr:'12121212',
						// cover:'h'
						// plnr:'12',
						plnr: that.zzpl,
						// zzpl:'',
					},
					dataType: 'json',
					success: res => {
						console.log(res.data)
						// this.zzpl = zzpl
						console.log('success')

					},
					fail: () => {

					},
					complete: () => {

					}
				})
			},
			AA() {
				uni.showToast({
					hdnr: '由于系统限制，您可以点击右上角•••分享',
					//将值设置为 success 或者直接不用写icon这个参数
					icon: 'none',
					//显示持续时间为 2秒
					duration: 2000
				})
			},
		}
	}
</script>

<style lang="scss">
	.tex {}

	.album {
		@include flex;
		align-items: flex-start;

		&__avatar {
			background-color: $u-bg-color;
			padding: 5px;
			border-radius: 3px;
		}

		&__content {
			margin-left: 8px;
			flex: 1;
		}
	}
</style>
