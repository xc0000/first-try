<template>
	<view>
		<u-row justify="space-between" customStyle="margin-bottom: 10px">
			<u-col span="6">
				<view style="padding: 20px;">
					<u--image :showLoading="true" :src="indexList.activitycover" width="250rpx" height="220rpx">
					</u--image>
				</view>
			</u-col>
			<u-col span="6">
				<view style="padding-bottom: 5rpx;"><text
						style="font-size: 20px;font-weight: 800;">{{indexList.activityname}}</text> </view>
				<view style="padding-bottom: 0rpx;"><text
						style="color: #0000ff;font-size: 12px;">{{indexList.activityperson}}<text
							style="color: #000000;">人参与</text></text> </view>
				<!-- <view style="padding-bottom: 5rpx;"><text style="color: #000000;font-size: 12px;">累计 <text
							style="color: #0000ff;font-size: 12px;">{{indexList.hdps}}</text><text
							style="color: #000000;">票</text></text></view> -->
				<!-- <view style="padding-bottom: 3rpx;"><text
						style="font-size: 14px;font-weight: 800;">{{indexList.hdzt}}</text> </view> -->
			</u-col>


		</u-row>
		<view style=" display: flex;flex-direction: column;justify-content: center;align-items: center;">
			<u-tabs :list="fl" :is-scroll="false" :current="current" bar-width="50" active-color="#30B1B7"
				@change="change"></u-tabs>
		</view>
		<view v-if="current == 0">
			<view class="u-page" style="margin: 2% 5% 2% 5%;">
				<view style="padding: 1px 0 5px 0;">
					<text style="font-size: 20px;font-weight: 800;">{{indexList.activityname}}</text>
				</view>
				<text style="font-size: 15px;">{{indexList.activitycontent}}</text>
				</text>
				<br>
				<u-tabbar @change="name => value6 = name" :fixed="true" :border="false" :placeholder="true"
					:safeAreaInsetBottom="true">
					<view style="width: 100%;padding: 0 5% 0 5%;">
						<u-row>
							<u-col span="10">
								<u-button type="primary" text="马上参加" @click="fabu"></u-button>
							</u-col>
							<u-col span="2">
								<view style="padding-left: 20%;">
									<u-icon name="share-square" size="70rpx" @click="btnClick()"></u-icon>
								</view>
							</u-col>
						</u-row>
					</view>
				</u-tabbar>

			</view>
		</view>
		<view v-if="current == 1">
			<view class="u-page">
				<u-list @scrolltolower="scrolltolower">
					<u-list-item v-for="(item, index) in rank" :key="index">
						<view @click="yh">
							<u-row>
								<u-col span="1">
									<view style="margin-left: 20px;">{{index + 1}}</view>
								</u-col>
								<u-col span="9">
									<u-cell :title="`${item.username}`" :label="`编号:${item.id}号`" :border="false">
										<u-avatar slot="icon" shape="square" size="35" :src="item.headimg"></u-avatar>
									</u-cell>
								</u-col>
								<u-col span="2">
									{{item.releasevote}}票
								</u-col>
							</u-row>
						</view>
					</u-list-item>
				</u-list>
			</view>
		</view>



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
</template>

<script>
	export default {
		data() {
			return {
				rank: [],
				indexList: [

				],
				urls: [
					'https://cdn.uviewui.com/uview/album/1.jpg',
					'https://cdn.uviewui.com/uview/album/2.jpg',
					'https://cdn.uviewui.com/uview/album/3.jpg',
					'https://cdn.uviewui.com/uview/album/4.jpg',
					'https://cdn.uviewui.com/uview/album/5.jpg',
					'https://cdn.uviewui.com/uview/album/6.jpg',
					'https://cdn.uviewui.com/uview/album/7.jpg',
					'https://cdn.uviewui.com/uview/album/8.jpg',
					'https://cdn.uviewui.com/uview/album/9.jpg',
					'https://cdn.uviewui.com/uview/album/10.jpg',
				],
				show: false,
				fl: [{
						name: '活动规则',
					},
					{
						name: '排行榜',
					}
				],
				current: 0,
				activityid: '',
			}
		},
		onLoad() {
			this.loadmore()
		},
		onShareAppMessage(res) {
			// 此处的distSource为分享者的部分信息，需要传递给其他人

			if (distSource) {
				return {
					title: '欢迎使用开发版本',
					type: 0,
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
					title: '欢迎使用开发版本',
					type: 0,
					query: 'id=' + distSource,
					summary: "",
					imageUrl: "https://img.js.design/assets/img/63146ed006e43e3200584b1a.jpg#1e9318d18f7da028f0943d1d5ccd1328"
				}
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数
			let that = this
			console.log('这是当前活动的id')
			console.log(option); //打印出上个页面传递的参数。
			that.activityid = option.activityid;
			this.activityid = option.activityid;
			uni.request({
				url: 'http://127.0.0.1:8000/getactivitydata/',
				header: {
					'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
				},
				method: 'POST',
				data: {
					activityid: this.activityid,
				},
				dataType: 'json',

				success: res => {
					console.log('当前页面获取的数据')
					this.indexList = res.data[0];
					console.log(res.data[0])

				},
				fail: () => {},
				complete: () => {}
			});
			this.getrank()

		},


		methods: {
			scrolltolower() {
				this.loadmore()

			},
			getrank() {
				console.log('当前页面获取的排行榜数据')
				uni.request({
					url: 'http://127.0.0.1:8000/activityrank/',
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
					},
					method: 'POST',
					data: {
						activityid: this.activityid,
					},
					dataType: 'json',

					success: res => {
						console.log('当前页面获取的排行榜数据')
						this.rank = res.data
						console.log(this.rank)

					},
					fail: () => {},
					complete: () => {}
				});
			},
			loadmore() {

			},
			change(index) {
				this.current = index.index
				//如报错则用this.current = index代替上行
			},
			close() {
				this.show = false
			},
			btnClick() {
				this.show = true;
			},
			AA() {
				uni.showToast({
					title: '由于系统限制，您可以点击右上角•••分享',
					//将值设置为 success 或者直接不用写icon这个参数
					icon: 'none',
					//显示持续时间为 2秒
					duration: 2000
				})
			},
			yh() {
				uni.navigateTo({
					url: `../activity-main/activity-main?id=${this.img}`
				});
			},
			fabu() {
				uni.navigateTo({
					url: `../fabu/fabu?activityid=${this.activityid}`
				})
			},

		}
	}
</script>

<style>

</style>
