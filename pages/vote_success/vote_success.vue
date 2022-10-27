<template>
	<view style="margin: 40% 0 60% 0;">
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;"><u-icon name="checkmark-circle-fill" color="#2979ff" size="250rpx"></u-icon><text style="font-size: 50rpx;font-weight: 700;">投票成功</text></view>
		<view style="display: flex;flex-direction: column;align-items: center;margin-bottom: 30%;">今日还剩{{piao}}次投票机会哦~</view>
		<view style="margin: 0% 20% 0% 20%;display: flex;flex-direction: column;align-items: center;margin-bottom: 5%;"><u-button type="primary" :plain="true" text="返回活动界面" @click="esc()"></u-button>
 
		</view>
       
      
	</view>
	
</template>

<script>
   
	export default {
		data() {
			return {
				piao:'',
			}
		},
		  onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
		     
		     let that = this
				console.log(option.id); //打印出上个页面传递的参数。
		         that.piao= option.id - 1;
				
		    },
		
		methods: {
		esc(){
			
			  uni.navigateBack({
			  		delta:1,//返回层数，2则上上页
			  	})
		},	
				
			 
			 
		}
	}
</script>

<style>

</style>
