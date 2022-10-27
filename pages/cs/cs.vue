<template>
	<view>
		<button @click="tq">1</button>
		<u-upload
				:fileList="fileList1"
				@afterRead="afterRead"
				@delete="deletePic"
				name="1"
				multiple
				:maxCount="10"
			></u-upload>
			{{fileList1}}
			{{namelist}}
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				hdnr:'12121212',
				img:'',
				userid:'1212',
				fabuid:'13',
					fileList1: [],
					namelist:[],

			}
		},
		methods: {
			tq() {
				let that=this
				uni.request({
					url: 'https://lin-undertake.cn/wxapi/DaKaFaBu/',	
					header: {
					'content-type': 'application/x-www-form-urlencoded;charset=utf-8', 
					}, 	
					method: 'POST',
					
					data: {
						userid:that.userid,
						
						hdnr:that.hdnr,
						cover:that.namelist
						},	
					// dataType: 'json',
												
					success: res => {
							console.log(res)
						},
					fail: () => {},
					complete:() => {}
				});
			},
				// 删除图片
						deletePic(event) {
							this[`fileList${event.name}`].splice(event.index, 1)
						},
						// 新增图片
						async afterRead(event) {
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
									message: '',
									url: result
								}))
								fileListLen++
							}
							let temp = this.fileList1[(this.fileList1.length)-1].name;
							console.log(temp)
							this.namelist.push(temp)
						},
						uploadFilePromise(url) {
							return new Promise((resolve, reject) => {
								let a = uni.uploadFile({
									url: 'https://lin-undertake.cn/wxapi/SaveGrImg/', // 仅为示例，非真实的接口地址
									filePath: url,
									name: 'file',
									formData: {
										user: 'test'
									},
									success: (res) => {
										setTimeout(() => {
											resolve(res.data.data)
										}, 1000)
									},
									
								});
								
							})
							
						},	
		}
	}
</script>

<style>

</style>
