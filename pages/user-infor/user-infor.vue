<template>
	<view>
		 <view class="pubBj" style="height: 300rpx;"></view>
		 
		 <view class=" mglr4">
			<view class="photoInfor flex">
			 	<view class="photo mgr5" @click="upLoadImg()">
					<image :src="submitData.mainImg&&submitData.mainImg[0]
					?submitData.mainImg[0].url:'../../static/images/about-img.png'" mode=""></image>
				</view>
			 	<view class="color6 fs12 mgt5">更改头像，设置个性化背景~</view>
			</view>
			<view class="flex pdtb15">
				<view class="fs18 ftw mgr15">{{mainData.info?mainData.info.name:''}}</view>
				<view class="flex ">
					<image style="width: 26rpx;height: 24rpx;" 
					src="../../static/images/zhuye-icon.png" mode=""></image>
					<!-- <text class="fs12 mgl5">新会员</text> -->
					<view class="fs12 mgl5" v-if="mainData.info&&mainData.info.level==0">新会员</view>
					<view class="fs12 mgl5" v-if="mainData.info&&mainData.info.level==1">普通</view>
					<view class="fs12 mgl5" v-if="mainData.info&&mainData.info.level==2">银卡</view>
					<view class="fs12 mgl5" v-if="mainData.info&&mainData.info.level==3">金卡</view>
					<view class="fs12 mgl5" v-if="mainData.info&&mainData.info.level==4">钻石卡</view>
				</view>
			</view>
		 </view>
		 <view class="f5H5"></view>
		 <view class="editLine fs13 pdlr4" >
		 	<view class="item flex">
				<view class="ll">会员名称</view>
				<view class="rr color6">{{mainData.info?mainData.info.name:''}}</view>
			</view>
			<view class="item flex">
				<view class="ll">手机号</view>
				<view class="rr color6">{{mainData.login_name}}</view>
			</view>
			<view class="item flex">
				<view class="ll">性别</view>
				<view class="rr color6" v-if="mainData.info&&mainData.info.gender==1">男</view>
				<view class="rr color6" v-if="mainData.info&&mainData.info.gender==2">男</view>
			</view>
			<view class="item flex">
				<view class="ll">年龄</view>
				<view class="rr color6">{{mainData.info?mainData.info.age:''}}</view>
			</view>
			<view class="item flex">
				<view class="ll">生日</view>
				<view class="rr color6">{{mainData.info?mainData.info.birth:''}}</view>
			</view>
		 </view>
		 
		 <view class="submitbtn mgt30">
			 <button class="btn" type="button" @click="Router.redirectTo({route:{path:'/pages/user-inforEdit/user-inforEdit'}})">完善会员资料</button>
		 </view>
		 
		 
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				mainData:{},
				submitData:{
					mainImg:[]
				}
				
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			upLoadImg(type) {
				const self = this;			
				wx.showLoading({
					mask: true,
					title: '上传中',
				});
				const callback = (res) => {
					console.log('res', res)
					if (res.solely_code == 100000) {
						self.submitData.mainImg = [];
						self.submitData.mainImg.push({url:res.info.url,type:'image'})
						console.log(self.submitData)
						wx.hideLoading()
						self.userInfoUpdate()
					} else {
						self.$Utils.showToast('网络故障', 'none')
					}
				};				
				wx.chooseImage({
					count: 1,
					success: function(res) {
						console.log(res);
						var tempFilePaths = res.tempFilePaths[0];
						console.log(callback)
						self.$Utils.uploadFile(tempFilePaths, 'file', {
							tokenFuncName: 'getProjectToken',
							type:'image'
						}, callback)
					},
					fail: function(err) {
						wx.hideLoading();
					},			
				})			
			},
			
			userInfoUpdate() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.data = {};
				postData.data = self.$Utils.cloneForm(self.submitData);
				const callback = (data) => {				
					if (data.solely_code == 100000) {					
						
					} else {
						uni.setStorageSync('canClick', true);
						self.$Utils.showToast(data.msg, 'none', 1000)
					}	
				};
				self.$apis.userInfoUpdate(postData, callback);
			},
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.submitData.mainImg = self.mainData.info.mainImg
						self.mainData.info.birth = self.$Utils.timeto(self.mainData.info.birth*1000,'ymd')
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userGet(postData, callback);
			},
			
			
			
		}
	};
</script>

<style>
.photoInfor .photo{width: 120rpx; height: 120rpx; position: relative;margin-top: -60rpx;}
.photoInfor .photo image{width: 100%; height: 120rpx;border-radius: 50%;}

.editLine .item .ll{width: 20%;}
.editLine .item .rr{width: 80%;}
.editLine .item{border-bottom: 1px solid #eee;padding: 30rpx 0;}
</style>

