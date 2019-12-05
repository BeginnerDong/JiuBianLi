<template>
	<view>
		 <view class="pubBj" style="height: 300rpx;"></view>
		 
		 <view class=" mglr4">
			<view class="photoInfor flex">
			 	<view class="photo mgr5"><image src="../../static/images/about-img.png" mode=""></image></view>
			 	<view class="color6 fs12 mgt5">更改头像，设置个性化背景~</view>
			</view>
			<view class="flex pdtb15">
				<view class="fs18 ftw mgr15">快乐的猫</view>
				<view class="flex ">
					<image style="width: 26rpx;height: 24rpx;" src="../../static/images/zhuye-icon.png" mode=""></image>
					<text class="fs12 mgl5">新会员</text>
				</view>
			</view>
		 </view>
		 <view class="f5H5"></view>
		 <view class="editLine fs13 pdlr4" >
		 	<view class="item flex">
				<view class="ll">会员名称</view>
				<view class="rr color6">快乐的猫</view>
			</view>
			<view class="item flex">
				<view class="ll">手机号</view>
				<view class="rr color6">15623562355</view>
			</view>
			<view class="item flex">
				<view class="ll">性别</view>
				<view class="rr color6">男</view>
			</view>
			<view class="item flex">
				<view class="ll">年龄</view>
				<view class="rr color6">0</view>
			</view>
			<view class="item flex">
				<view class="ll">生日</view>
				<view class="rr color6">1993年11月23日</view>
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
				showView: false,
				wx_info:{},
				is_show:false
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			getMainData() {
				const self = this;
				console.log('852369')
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				self.$apis.orderGet(postData, callback);
			}
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

