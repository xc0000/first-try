<template>
 
<!--  <view style="padding: 50% 0 50% 0;">
  <u-empty
          icon="http://cdn.uviewui.com/uview/empty/order.png"
    text="您还没有投票过哦，快去参与活动吧~"
    textSize="15px"
  >
  </u-empty>
  <view style="padding: 20px 35% 0 35%;"><u-button type="primary" :plain="true" shape="circle" text="参加活动" @click="official()"></u-button></view> 
 </view> -->
</template>

<script>
 export default {
  data() {
   return {
    
   }
  },
  onLoad: function (option) { //option为object类型，会序列化上个页面传递的参数
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
             url: 'https://lin-undertake.cn/wxapi/GeRenZhuYe-toupiao/', 
             header: {
             'content-type': 'application/x-www-form-urlencoded;charset=utf-8', 
             },  
             method: 'POST',
             
             data: {
              userid:res.data,
              
              
              }, 
             // dataType: 'json',
                    
             success: res => {
               console.log(res)
               that.article = res.data['hdxx'];
               console.log(res.data['hdxx'])
              },
             fail: () => {},
             complete:() => {}
            });
            
           }
          });
         }
        }
       });
    },
  methods: {
   official() {
    uni.navigateTo({
        url: '../official/official'
    })
   },
  }
 }
</script>

<style>

</style>