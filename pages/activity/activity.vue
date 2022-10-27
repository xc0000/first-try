<template>
	<view>
		<view style="margin: 5px 15rpx 5px 15rpx;">
			<!--  搜索框margin 属性为（四个）上下左右方向的外边距属性 指定四个值 依次为 上 右 下 左的外边距即上 右 下 左的框的边的长 -->
			<u-search shape="square" :animation="true"></u-search>
			<!-- u-search 表示搜索框的组件  square 表示形状为方形 animation表示是否开启动画-->
		</view>


		<view style="margin: 0 15rpx 0 15rpx;">
			<!-- 轮播图的外框的 外边距的上 右 下 左 的长度 -->
			<u-swiper :list="propaganda" @click="official"></u-swiper>
			<!--@click="official"详细内容 -->
			<!-- 表示获取轮播图的数据 并且可点击 -->
			<u-row customStyle="margin-bottom: 10px;background-color: #f9f9f9;border-radius: 7px;">
				<!--设置分栏的外部样式 margin-bottom表示下外边距长 背景颜色为 border-radius表示设置边框圆角的弧度 -->
				<u-col span="10" @click="official">
					活动入口
					<!-- 通过col组件的span设置分栏的比例 -->
				</u-col>
				<u-col span="2" @click="official">
					<!-- 设置活动入口该框的分栏分两栏 并可点击 -->
					<u-icon name="arrow-right" size="15" label="更多" labelPos="left"></u-icon>
					<!-- 设置该行的图标名称 大小为15 右侧label的文字为更多 label相对于图标的位置在左 -->
				</u-col>

			</u-row>
		</view>

		<view>
			<view class="u-demo-block__content" v-for="(item,index) in article" :key="index" :label="item">
				<!--  -->
				<view class="" v-if="index%2==0">
					<!-- 如果能被2整除 -->
					<u-row customStyle="margin-bottom: 10px">
						<!-- 设置分栏的外部样式 margin-bottom 表示下外边距为 10px  -->
						<u-col span="6">
							<!-- 设置分栏数为6 列 -->
							<view class="vi" @click="main(index)">
								<u--image :showLoading="true" :src="imglist[index]" width="100%" height="130px">
								</u--image>
								<!-- {{item.urls1.img[0]}} -->
								<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
									#{{item.activityname}}</view>
								<!-- font-size表示指定文字的大小  padding-top表示内边距区域中的上方的高度  padding-left表示内边距区域中左方的距离 并且文字的颜色为 再进行数据绑定显示文字 -->
								<view style="font-size: 13px;padding-top: 2px;padding-left:5px;">{{item.releasecontent}}
								</view>
								<!-- font-size 设置字体大小 padding-top表示内边距区域中的上方高度padding-left表示内边距区域的左方的长度 并进行数据绑定 显示标题文字 -->
								<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
									<!-- font-size 设置字体大小 padding-top 表示内边距区域中的上方高度 padding-left表示内边距区域的左方的长度 -->
									<u-row customStyle="margin-bottom: 10px;">

										<u-col span="1">
											<u-icon name="account-fill" size="12"></u-icon>
											<!-- 表示显示用户对应的图标 并且大小为12  -->
										</u-col>

										<u-col span="6">
											{{article[index].username}}
											<!-- 数据绑定 显示用户的名字 -->
										</u-col>

										<u-col span="1">
											<view>
												<u-icon name="coupon-fill" size="13"></u-icon>
											</view>
											<!-- 设置投票的图标的大小 -->
										</u-col>

										<u-col span="4">
											<view style="flex: 1;text-align: left;">{{article[index].releasevote}}票
											</view>
											<!-- flex设置了弹性项目如何增大或缩小以适应弹性容器中可用的空间 text-align表示定义行中文字如何相对它的块父元素进行对齐  显示数据绑定的票数-->
										</u-col>
									</u-row>
								</view>
							</view>
						</u-col>
						<u-col span="6" v-if="index+1<article.length">
							<!-- 如果该<文章的长度 -->
							<view class="vi" @click="main(index+1)">
								<!-- 设置可点击 -->
								<u--image :showLoading="true" :src="imglist[index+1]" width="100%" height="130px">
								</u--image>
								<!-- 显示它的图片 并设置宽度 以及高度 -->
								<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">
									#{{article[index+1].activityname}}</view>
								<!-- 设置参加的大活动分文字的大小 padding-top设置内边距区域上方的高度 padding-left表示设置内边距区域左方的长度 并设置文字的颜色 进行数据绑定显示文字 -->
								<view style="font-size: 13px;padding-top: 2px;padding-left:5px;">
									{{article[index+1].releasecontent}}
								</view>
								<!-- 设置自己设置的活动的文字的大小 padding-top表示设置内边距区域上方的高度 padding-left表示设置内边距区域的左方长度 并进行数据绑定文字 -->
								<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
									<!--设置文字的大小 padding-top表示设置内边距区域上方的高度 padding-left表示设置内边距区域的左方的长度   -->
									<u-row customStyle="margin-bottom: 10px;">
										<!--  -->
										<u-col span="1">
											<u-icon name="account-fill" size="12"></u-icon>
											<!-- 设置用户的图标 以及图标的大小  -->
										</u-col>
										<u-col span="6">
											{{article[index+1].username}}
											<!-- 进行数据绑定设置用的名称 -->
										</u-col>
										<u-col span="1">
											<u-icon name="coupon-fill" size="13"></u-icon>
											<!-- 设置投票对应的图标并设置图标的大小 -->
										</u-col>

										<u-col span="4">
											<view style="flex: 1;text-align: left;">{{article[index+1].releasevote}}票
											</view>
											<!-- flex设置了弹性项目如何增大或缩小以适应弹性容器中可用的空间 tezt-align表示行中文字如何相对于它的块父元素对齐 并进行数据绑定设置票数 -->
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


</template>

<script>
	export default {

		data() {
			return {
				// one:'',
				article: [],
				imglist: [],
				propaganda: [
					'https://cdn.uviewui.com/uview/swiper/swiper1.png',
					'https://cdn.uviewui.com/uview/swiper/swiper2.png',
					'https://cdn.uviewui.com/uview/swiper/swiper3.png',
				],

			}
		},

		onLoad: function(e) {
			uni.request({
				url: 'http://127.0.0.1:8000/postreleasedata/',
				method: 'GET',
				data: {
					// userid:this.data
				},
				success: res => {
					this.article = res.data
					let w = 0;
					var q = '';
					console.log(this.article)
					for (let i = 0; i < this.article.length; i++) {
						let a = this.article[i]['releasepicture']
						if (a == '') {
							this.imglist.push('')
							continue
						}
						for (let j = 0; j < a.length; j++) {
							// console.log(this.info['releasepicture'][i]);
							if (a[j] == '"' && w == 1) {
								console.log(q);
								this.imglist.push('http://127.0.0.1:8000/upload/' + q)
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
				}

			})

		},

		methods: {
			main(w) {
				w = this.article[w].releaseid
				console.log('第', w, '个')

				uni.navigateTo({
					url: `../activity-main/activity-main?id=${w}`,
					// 设置活动入口下方的活动点击进入文章详情页面
				})
			},
			official() {
				uni.navigateTo({
					url: '../official/official',
					// 设置轮播图跳转到主题活动页面   接口 
				})
			},
			up() {
				uni.navigateTo({
					url: '../fabu/fabu',
					// 设置官方活动页面 点击马上参与 跳转到发布页面 接口
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
		/* 设置vi 区域的 padding表示上 右 下 左的长度 ；margin表示设置上下左右的外边距的长度；并设置背景颜色 */
		/* border-radius 表示允许你设置的外边距的圆角 ； align-items表示设置纵轴上如何排列已处理空白区域*/
		/* box-shadow表示 x偏移量 y偏移量 阴影模糊半径 阴影颜色 z-index表示设定了一个定位元素及其后代元素*/
	}
</style>
