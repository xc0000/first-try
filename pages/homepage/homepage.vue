<template>
	<view>
		<view class="content-item">
			<view style="width:100%;height:120px;background-color: #ffff55">

			</view>
			<view class="user-info">
				<view>
					<u-avatar size="60" :src="avatarUrl"></u-avatar>
				</view>
				<view class="name">
					<view>

						<view style="padding-left: 20px;">
							<u-cell-group :border="false">
								<u-cell :border="false" :title="`${nickName}`"></u-cell>
							</u-cell-group>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view style=" display: flex;flex-direction: column;justify-content: center;align-items: center;">
			<u-tabs :list="fl" :is-scroll="false" :current="current" bar-width="50"
				itemStyle=" padding: 0px 150rpx  20rpx 150rpx;&.is-active{};height: 60px;" active-color="#30B1B7"
				@change="change"></u-tabs>
		</view>
		<view v-if="current == 0">
			<view>
				<view class="u-demo-block__content" v-for="(item,index) in article" :key="index" :label="item">
					<view class="" v-if="index%2==0">
						<u-row customStyle="margin-bottom: 10px">
							<u-col span="6">

								<view class="vi" @click="main(index)">
									<u--image :showLoading="true" :src="imglist[index]" width="100%" height="130px">
									</u--image>
									<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
										#{{article[index].activityname}}</view>
									<view style="font-size: 15px;padding-top: 2px;padding-left:5px;">
										{{article[index].releasecontent}}...
									</view>
									<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
										<u-row customStyle="margin-bottom: 10px;">
											<u-col span="1">
												<u-icon name="account-fill" size="12"></u-icon>
											</u-col>
											<u-col span="6">
												{{nickName}}
											</u-col>
											<u-col span="1">
												<view>
													<u-icon name="coupon-fill" size="13"></u-icon>
												</view>
											</u-col>
											<u-col span="4">
												<view style="flex: 1;text-align: left;">{{article[index].hdps}}票</view>
											</u-col>
										</u-row>
									</view>
								</view>
							</u-col>
							<u-col span="6" v-if="index+1<article.length">
								<view class="vi" @click="main(index+1)">
									<u--image :showLoading="true" :src="imglist[index+1]" width="100%" height="130px">
									</u--image>
									<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
										#{{article[index+1].activityname}}</view>
									<view style="font-size: 15px;padding-top: 2px;padding-left:5px;">
										{{article[index+1].releasecontent}}...
									</view>
									<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
										<u-row customStyle="margin-bottom: 10px;">
											<u-col span="1">
												<u-icon name="account-fill" size="12"></u-icon>
											</u-col>
											<u-col span="6">
												{{article[index+1].username}}
											</u-col>
											<u-col span="1">
												<u-icon name="coupon-fill" size="13"></u-icon>
											</u-col>

											<u-col span="4">
												<view style="flex: 1;text-align: left;">{{article[index].hdps}}票</view>
											</u-col>
										</u-row>
									</view>
								</view>
							</u-col>
						</u-row>
					</view>
				</view>
			</view>
		</view>
		<view v-if="current == 1">
			<view style="padding: 25% 0 75% 0;">

				<u-empty mode="coupon" icon="http://cdn.uviewui.com/uview/empty/coupon.png" text="您还没有商品哦~"
					textSize="15px">
				</u-empty>

			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				nickName: uni.getStorageSync('name'),
				avatarUrl: uni.getStorageSync('img'),
				openid: "",
				article: [

				],
				imglist: [],
				fl: [{
						name: '活动',
					},
					{
						name: '商品',
					}
				],
				current: 0,
			}
		},

		onLoad() {
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
								that.openid = res.data;
								console.log('openid:' + res.data);
								console.log('获取成功')
								uni.request({
									url: 'http://127.0.0.1:8000/postreleaseidalldata/',
									header: {
										'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
									},
									method: 'POST',

									data: {
										openid: res.data,
									},
									// dataType: 'json',

									success: res => {
										that.article = res.data
										let w = 0;
										var q = '';
										console.log(that.article)
										for (let i = 0; i < that.article.length; i++) {
											let a = that.article[i]['releasepicture']
											if (a == '') {
												that.imglist.push('')
												continue
											}
											for (let j = 0; j < a.length; j++) {
												// console.log(this.info['releasepicture'][i]);
												if (a[j] == '"' && w == 1) {
													console.log(q);
													that.imglist.push(
														'http://127.0.0.1:8000/upload/' +
														q)
													w = 0;
													q = '';
													break;
												}
												if (a[j] == '"' && w == 0) {
													w = 1;
													continue;
												}
												if (a[j] != '"' && w == 1) {
													q = q + a[j];
													continue;
												}
											}
										}
									},
									fail: () => {},
									complete: () => {}
								});

							}
						});
					}
				}
			});

		},
		methods: {
			change(index) {
				this.current = index.index
				//如报错则用this.current = index代替上行
			},
			main(w) {
				w = this.article[w].releaseid
				console.log('第', w, '个')

				uni.navigateTo({
					url: `../activity-main/activity-main?id=${w}`,
					// 设置活动入口下方的活动点击进入文章详情页面
				})
			},
		}
	}
</script>

<style>
	.container {
		width: 100%;
	}

	.profile {
		width: 100%;
	}

	.user-info {
		position: absolute;
		top: 30px;
		left: 30px;
		display: flex;
		align-items: center;

	}

	.vi {
		padding: 2rpx 0rpx 2rpx 0rpx;
		margin: 20rpx 15rpx 0rpx 15rpx;
		background-color: #f9f9f9;
		border-radius: 7px;
		align-items: center;
		box-shadow: 0px 0px 10px #ffffff;
		z-index: 1;
	}

	.user-info .name {
		display: flex;
	}

	.content-item {
		display: flex;

		flex-direction: column;
		border-radius: 4px;
		background-color: #FFFFFF;
		justify-content: space-around;
	}

	.content-item .title {
		padding: 15px;
		color: #5A5B5C;
		font-size: 16px;
		font-weight: bold;
	}

	.content-show {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
	}

	.money-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20px;
	}

	.service-item {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 20px;
	}

	.service-item .service-text {
		font-size: 12px;
		color: #919293;
	}

	.order-tab {
		display: flex;
		flex-direction: column;
		align-items: center;
		padding: 15px;
	}

	.order-title {
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
