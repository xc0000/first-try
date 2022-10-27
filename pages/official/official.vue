<template>
	<view>
		<view class="u-demo-block__content" v-for="(item,index) in article" :key="index" :label="item">
			<view class="" v-if="index%2==0">
				<u-row customStyle="margin-bottom: 10px">
					<u-col span="6">
						<view class="vi" @click="main(index)">
							<!--@click="main" 详细内容 -->
							<u--image :showLoading="true" :src="article[index].activitycover" width="100%"
								height="120px">
							</u--image>
							<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
								<u-row customStyle="margin-bottom: 10px">
									<u-col span="7">
										<view style="font-size: 15px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
											#{{article[index].activityname}}</view>
									</u-col>
									<u-col span="1">
										<u-icon name="man-add-fill" color="#ff0000" size="16"></u-icon>
									</u-col>
									<u-col span="2">
										<view style="font-size: 15px;">{{article[index].activityperson}}</view>
									</u-col>
								</u-row>
							</view>
						</view>
					</u-col>
					<u-col span="6" v-if="index+1<article.length">
						<view class="vi" @click="a(index+1)">
							<view class="">
								<!-- {{article[index+1].id}} -->
							</view>
							<u--image :showLoading="true" :src="article[index+1].activitycover" width="100%"
								height="120px">
							</u--image>
							<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
								<u-row customStyle="margin-bottom: 10px">
									<u-col span="7">
										<view style="font-size: 15px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
											#{{article[index+1].activityname}}</view>
									</u-col>
									<u-col span="1">
										<u-icon name="man-add-fill" color="#ff0000" size="16"></u-icon>
									</u-col>
									<u-col span="2">
										<view style="font-size: 15px;">{{article[index+1].activityperson}}</view>
									</u-col>
								</u-row>
							</view>
						</view>
					</u-col>
				</u-row>
			</view>
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				article: [

				],

			}
		},
		//下拉刷新
		onPullDownRefresh() {
			setTimeout(() => {
				uni.request({
					url: 'http://127.0.0.1:8000/getactivitydata/',
					method: 'GET',
					// data:Qs.stringify(this.usr.userid),
					success: res => {
						this.article = res.data;
						console.log(this.article)
					},
					fail: () => {},
					complete: () => {}
				});
			}, 400)


		},

		onLoad() {
			uni.request({
				url: 'http://127.0.0.1:8000/getactivitydata/',
				method: 'GET',
				// data:Qs.stringify(this.usr.userid),
				success: res => {
					this.article = res.data;
					console.log('这是请求所有官方活动详情数据')
					console.log(res.data)
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			main(w) {
				w = this.article[w].activityid
				console.log('第', w, '个')
				uni.navigateTo({
					url: `../gfhd/gfhd?activityid=${w}`
				})

			},
			a(w) {
				w = this.article[w].activityid
				console.log('第', w, '个')
				uni.navigateTo({
					url: `../gfhd/gfhd?activityid=${w}`
				})

			},
		},
	}
</script>

<style>
	.vi {
		padding: 2rpx 0rpx 2rpx 0rpx;
		margin: 20rpx 15rpx 0rpx 15rpx;

		background-color: #f9f9f9;

		border-radius: 7px;
		align-items: center;
		box-shadow: 0px 0px 10px #ffffff;
		z-index: 1;
	}
</style>
