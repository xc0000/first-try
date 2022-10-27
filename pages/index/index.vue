<!-- <template>
	<view class="content">
		<u-empty
		        mode="r"
		        icon="https://img.js.design/assets/img/63146ed006e43e3200584b1a.jpg#1e9318d18f7da028f0943d1d5ccd1328"
		>
		</u-empty>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		 <view style="width:40%;padding: 5%;"><u-button :plain="true" type="primary" size="small" text="分享给别人" open-type="share" ></u-button></view>
		  <view style="width:40%;padding: 5%;"><u-button :plain="true" type="primary" size="small" text="测试" @click="123" ></u-button></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '在改了，快改完了'
			}
		},
		onLoad(){
			
			wx.showShareMenu({
				withShareTicket:true,
				//设置下方的Menus菜单，才能够让发送给朋友与分享到朋友圈两个按钮可以点击
				menus:["shareAppMessage","shareTimeline"]
			})
		},
		//发送给朋友
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
  
		methods: {
123() {
				uni.navigateTo({
				    url: '../123/123'
				})
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding-top: 200px;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
 -->
 <template>
 	<view>
 			<view style="margin: 5px 15rpx 5px 15rpx;"> <u-search shape="square" :animation="true"></u-search></view> 
 			<view style="margin: 0 15rpx 0 15rpx;">
 			  <u-swiper :list="propaganda" @click="official"></u-swiper>  <!--@click="official"详细内容 -->
 			  <u-row customStyle="margin-bottom: 10px;background-color: #f9f9f9;border-radius: 7px;" >
 			  									<u-col span="10" @click="official">
 			  										活动入口
 			  									</u-col>
 												<u-col span="2" @click="official">
 														<u-icon name="arrow-right"size="15" label="更多" labelPos="left"></u-icon> 
 												</u-col>
 			  									
 			   </u-row>
 			</view>
 			
 			<view>
 				<view class="u-demo-block__content" v-for="(item,index) in article" :key="index"  :label="item">
 				    <view class="" v-if="index%2==0">
 				    	<u-row customStyle="margin-bottom: 10px" >
 				    	    <u-col span="6">
 				    			
 				    	        <view class="vi" @click="main" > 
 				    	    		<u--image :showLoading="true" :src="item.cover" width="100%" height="130px" ></u--image>
 				    	    		<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">#{{item.type}}</view>
 				    	    		<view style="font-size: 15px;padding-top: 2px;padding-left:5px;">{{item.title}}...</view>
 				    	    		<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
 				    	    		  <u-row customStyle="margin-bottom: 10px;">
 				    	    			<u-col span="1">
 				    	    				<u-icon name="account-fill"size="12" ></u-icon>
 				    	    			</u-col>
 				    	    			<u-col span="6">
 				    	    				{{article[index].user}}
 				    	    			</u-col>
 				    	    			<u-col span="1">
 				    	    				<view ><u-icon name="coupon-fill" size="13" ></u-icon></view>
 				    	    			</u-col>
 				    	    			<u-col span="4">
 				    	    			<view style="flex: 1;text-align: left;">{{article[index].eye}}票</view>
 				    	    			</u-col>
 				    	               </u-row>
 				    	    		</view>
 				    			</view>
 				    	    </u-col>      
 				    	     <u-col span="6" v-if="index+1<article.length">
 				    	         <view class="vi" @click="main" >
 				    	         	<u--image :showLoading="true" :src="item.cover" width="100%" height="130px" ></u--image>
 				    	         	<view style="font-size: 12px;padding-top: 2px;padding-left:5px;color: #ffaa00;">#{{item.type}}</view>
 				    	         	<view style="font-size: 15px;padding-top: 2px;padding-left:5px;">{{item.title}}...</view>
 				    	         	<view style="font-size: 12px;padding-top: 5px;padding-left:5px;">
 				    	         	  <u-row customStyle="margin-bottom: 10px;">
 				    	         		<u-col span="1">
 				    	         			<u-icon name="account-fill"size="12" ></u-icon>
 				    	         		</u-col>
 				    	         		<u-col span="6">
 				    	         			{{article[index+1].user}}
 				    	         		</u-col>
 				    	         		<u-col span="1">
 				    	         			<u-icon name="coupon-fill" size="13" ></u-icon>
 				    	         		</u-col>
 										
 				    	         		<u-col span="4">
 				    	         			<view style="flex: 1;text-align: left;">{{article[index].eye}}票</view>
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
 				article:[
 				    {
 				   "eye":'10000',
 				   "user":'用户',
 				   "type":'标签内容',
 				   "title":' 杭州周边绝美浆板露营',
 				   "cover": 'https://cdn.uviewui.com/uview/album/3.jpg',    
 				    },
 				    {
 				   "eye":'122',
 				   "user":'用户1',
 				   "type":'标签内容1',
 				   "title":' 杭州周边绝美浆板露营1',
 				   "cover": 'https://cdn.uviewui.com/uview/album/3.jpg',    
 				     },
 					 {
 					 "eye":'4',
 					 "user":'用户1',
 					 "type":'标签内容1',
 					 "title":' 杭州周边绝美浆板露营1',
 					 "cover": 'https://cdn.uviewui.com/uview/album/3.jpg',    
 					   },
 				     
 				],
 			        
                 propaganda: [
                     'https://cdn.uviewui.com/uview/swiper/swiper1.png',
                     'https://cdn.uviewui.com/uview/swiper/swiper2.png',
                     'https://cdn.uviewui.com/uview/swiper/swiper3.png',
                 ],
 				
             } 
         },
 		methods: {
 				 main() { 
 				 	uni.navigateTo({
 				 	    url: '../activity-main/activity-main'
 				 	})
 				 },
 				 official() {
 				 	uni.navigateTo({
 				 	    url: '../official/official'
 				 	})
 				 },
 				 up() {
 				 	uni.navigateTo({
 				 	    url: '../fabu/fabu'
 				 	})
 				 },
 				
 		},
     }
 </script>
 
 <style>
 .vi {
 	padding:2rpx 0rpx 2rpx 0rpx;
 	margin:20rpx 15rpx 0rpx 15rpx;
 	background-color:#f9f9f9;
 	border-radius:7px;
 	align-items:center;
 	box-shadow:0px 0px 10px #ffffff;
 	z-index:1;
 }
 </style>
 