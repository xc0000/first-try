<template >
	<view>
		<u-cell-group>
			<u-cell
			    title="关于"
			    isLink
			    url="/pages/myrelease/myrelease"
			></u-cell>
			{{currentSize}}
		</u-cell-group>
		<view style="background-color: #bfbfbf;margin-top: 5%;"><u-button type="primary" :plain="true" :hairline="true" color="#bfbfbf" text="退出登录" @click="q"></u-button></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
					
			}
		},
		onLoad() {
				
			},

		methods: {
		q(){
			
			    uni.clearStorageSync('name');

		},	
		}
	}
</script>

<style>

</style>
