<template>
	<view style="margin: 40% 0 60% 0;">
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;"><u-icon name="close-circle-fill" color="#554b52" size="250rpx"></u-icon><text style="font-size: 50rpx;font-weight: 700;">投票失败</text></view>
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 30%;">今日投票机会已用完，明天再来吧~</view>
		<view style="margin: 0% 20% 0% 20%;display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;"><u-button type="primary" :plain="true" text="返回活动界面" @click="esc()"></u-button>
 
		</view>
       
      
	</view>
	
</template>

<script>
   
	export default {
		data() {
			return {
			
			}
		},

		
		methods: {
		esc(){
			
			  uni.navigateBack({
			  		delta:2,//返回层数，2则上上页
			  	})
		},	
				
			 
			 
		}
	}
</script>

<style>

</style>
