<template>
	<view>
		<view class="pdlr4 pubBj hbCont">
			
			<view class="bigTit center white">
				<!-- <view class="fs20 tit">双十一</view> -->
				<view class="tex">满减红包大派送</view>
			</view>
			
			<view class="quanList center flexRowBetween pubColor">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="title">先领券后购物</view>
					<view class="infor">
						<view class="price">{{item.value}}</view>
						<view class="tt">·满{{item.condition}}元使用·</view>
						<!-- <view class="text fs12">({{item.lable}})</view> -->
					</view>
				</view>
			</view>
			
			<view class="flexCenter pdtb15">
				<view class="getBtn pubColor center fs16" @click="Router.navigateTo({route:{path:'/pages/hongbao_Coupon/hongbao_Coupon'}})">一键领取</view>
			</view>
			
			
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
				is_show:false,
				quanList:[
					{price:'5',infor:'满50元使用',lable:'香烟除外'},
					{price:'10',infor:'满100元使用',lable:'香烟除外'},
					{price:'15',infor:'满200元使用',lable:'香烟除外'},
					{price:'20',infor:'满300元使用',lable:'香烟除外'}
				],
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						is_page: true,
						pagesize: 10
					}
				};
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	page{background: #FF2121;padding-bottom: 60rpx;}
	.hbCont{background: url(../../static/images/hongbao-img.png) no-repeat center 0/713rpx 411rpx;}
	.bigTit{width: 713rpx;height: 411rpx; margin: 0 auto;text-shadow: 0 4rpx 10rpx rgba(0,0,0,0.2);box-sizing: border-box;padding-top: 50rpx;}
	.bigTit .tit{font-size: 80rpx; line-height: 80rpx;margin-bottom: 30rpx;letter-spacing: 16rpx;}
	.bigTit .tex{font-size: 52rpx; line-height: 52rpx;letter-spacing: 8rpx;}
	
	.quanList{flex-wrap: wrap;padding: 0 2%;}
	.quanList .item{width: 315rpx;background: rgba(255,255,255,0.5);padding-bottom: 30rpx; margin-bottom: 30rpx;}
	.quanList .item .title{line-height: 100rpx;height: 100rpx;color: #fff;font-size: 34rpx;font-weight: bold;}
	.quanList .item .infor{width: 290rpx;margin: 0 auto; background: #fff;padding:10rpx 0 20rpx 0;}
	.quanList .item .infor .price{font-size:100rpx;color: #FF2121; line-height: 120rpx;font-weight: bold;}
	.quanList .item .infor .price:before{font-size: 32rpx; line-height: 20px}
	
	.getBtn{width: 300rpx;height: 80rpx;line-height: 80rpx;border-radius: 40rpx;background: #f8ad14;box-shadow: 0 2rpx 10rpx rgba(0,0,0,0.2);}
	
	.xqInfor view{padding-bottom: 10rpx;}
	
</style>

