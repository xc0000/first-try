<template>
	<view style="margin: 2% 5% 2% 5%;">
		<u-upload :fileList="fileList1" @afterRead="afterRead" @delete="deletePic" name="1" multiple :maxCount="10">
		</u-upload>
		<view>

			<u--textarea v-model="activitycontent" placeholder="请输入内容" border="bottom"></u--textarea>
		</view>
		<u-notify ref="uNotify" message="Hi uView"></u-notify>
		<u-tabbar @change="name => value6 = name" :fixed="true" :border="false" :placeholder="true"
			:safeAreaInsetBottom="true">
			<view style="width: 100%;padding: 0 5% 0 5%;">
				<u-button type="primary" text="发布" @click="submit"></u-button>
			</view>
		</u-tabbar>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				indexList: [],
				activitycontent: '',
				activityid: '',
				img: '',
				tempimgurl: '',
				imgURL: '',

				fileList1: [],
				show: false,
				namelist: [],
				userinfo: {
					openid: '',
				},
				imglist: []
			}
		},
		onLoad: function(option) { //option为object类型，会序列化上个页面传递的参数

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
			console.log('打印出上个页面传递的参数。');
			this.activityid = option.activityid;
			console.log(this.activityid); //打印出上个页面传递的参数。
		},
		methods: {


			submit() {

				let that = this
				uni.request({
					url: 'http://127.0.0.1:8000/postreleasedata/',
					header: {
						'content-type': 'application/x-www-form-urlencoded;charset=utf-8',
					},
					method: 'POST',
					data: {
						openid: that.userinfo.openid,
						activityid: that.activityid,
						activitycontent: that.activitycontent,
						releasepicture: that.imglist
					},
					dataType: 'json',
					success: res => {
						console.log(res)
						this.$refs.uNotify.show({
							top: 10,
							type: 'error',
							color: '#000',
							bgColor: '#00ff7f',
							message: '发布成功',
							duration: 1000 * 3,
							fontSize: 20,
							safeAreaInsetTop: true
						})
						uni.navigateTo({
							url: '../homepage/homepage',
						})
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 删除图片
			deletePic(event) {
				this[`fileList${event.name}`].splice(event.index, 1)
			},
			// 新增图片
			async afterRead(event) {
				let that = this
				// 当设置 mutiple 为 true 时, file 为数组格式，否则为对象格式
				let lists = [].concat(event.file)
				let fileListLen = this[`fileList${event.name}`].length
				lists.map((item) => {
					this[`fileList${event.name}`].push({
						...item,
						status: 'uploading',
						message: '上传中'
					})

				})
				for (let i = 0; i < lists.length; i++) {
					const result = await this.uploadFilePromise(lists[i].url)
					let item = this[`fileList${event.name}`][fileListLen]
					this[`fileList${event.name}`].splice(fileListLen, 1, Object.assign(item, {
						status: 'success',
						message: '123',
						url: result
					}))
					console.log(this[`fileList${event.name}`][fileListLen].thumb);
					this.tempimgurl = this[`fileList${event.name}`][fileListLen].thumb;
					console.log(this.tempimgurl)
					fileListLen++
				}


			},
			uploadFilePromise(url) {
				return new Promise((resolve, reject) => {
					// 只能单个文件上传
					let a = uni.uploadFile({
						url: 'http://127.0.0.1:8000/postreleaseimgfile/', // 仅为示例，非真实的接口地址
						filePath: url,
						name: 'file',
						formData: {
							user: 'test',
						},
						success: (res) => {
							console.log('成功上传图片')
							setTimeout(() => {
								resolve(res.data.data)
							}, 1000)
							console.log(res.data)
							this.imglist.push(res.data)
							console.log(this.imglist)
						},

					});

				})

			},

		}

	}
</script>
