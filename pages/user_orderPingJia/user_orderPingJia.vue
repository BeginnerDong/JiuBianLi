<template>
	<view>
		<view class="proList proList-row mglr4 mgt15">
			<view class="item">
				<view class="flexRowBetween mglr4 pdtb10">
					<view class="fs12 color9">交易时间：2018-08-30</view>
					<view class="fs12 red">待确认</view>
				</view>
				<view class="flexRowBetween">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						
						<view class="flexRowBetween B-price">
							<view class="price fs14">56.00</view>
							<view class="flex">×1</view>
						</view>
					</view>
				</view>
				<view class="pdlr4">
					<view class="flexEnd pdtb15">共1件商品 合计:￥56</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4 whiteBj radius8 mgt15 pdlr4">
			<view class="pdtb15 flex borderB1">
				<view class="mgr10 fs13">星级评价</view>
				<view class="starBox mgr5">
					<image src="../../static/images/evaluation-icon1.png" mode=""></image>
					<image src="../../static/images/evaluation-icon.png" mode=""></image>
					<image src="../../static/images/evaluation-icon.png" mode=""></image>
					<image src="../../static/images/evaluation-icon.png" mode=""></image>
					<image src="../../static/images/evaluation-icon.png" mode=""></image>
				</view>
			</view>
			<view class="pdtb15 flexRowBetween" style="align-items: flex-start;">
				<view class="fs13  mgt5">填写评价</view>
				<view style="width: 80%;">
					<textarea class="fs12 f5bj radius8" style="height: 240rpx;" value="" placeholder="请写下您的宝贵意见" />
				</view>
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<view class="btn">确定</view>
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
	@import "../../assets/style/proList.css";
	@import "../../assets/style/star.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.proList-row .item .pic{width: 160rpx;height: 160rpx;}
	.proList-row .item .infor{height: 160rpx;width: 75%;}
	
	.starBox image{width: 28rpx;height: 28rpx;margin-right: 8rpx;}
</style>
