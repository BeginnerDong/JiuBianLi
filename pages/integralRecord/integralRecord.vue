<template>
	<view>
		<view class="orderNav flex fs14 whiteBj mgb10 color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待确认</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">配送中</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
		</view>
		<view class="pdlr4">
			<view class="proList proList-row">
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
						<div class="underBtn flexEnd pdb15">
							<span class="Bbtn">确认兑换</span>
						</div>
					</view>
				</view>
				<view class="item">
					<view class="flexRowBetween mglr4 pdtb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">配送中</view>
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
						<view class="underBtn flexEnd pdb15">
							<view class="Bbtn">确认收货</view>
						</view>
					</view>
				</view>
				<view class="item">
					<view class="flexRowBetween mglr4 pdtb10">
						<view class="fs12 color9">交易时间：2018-08-30</view>
						<view class="fs12 red">已完成</view>
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
				</view>
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
				current:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current
				}
			},
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
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav .tt{width: 25%;}
	.proList-row .item .pic{width: 160rpx;height: 160rpx;}
	.proList-row .item .infor{height: 160rpx;width: 75%;}
	
	.underBtn .Bbtn{display: block; padding: 0 20rpx; line-height: 50rpx; border:1px solid #FF2121; color: #FF2121;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 26rpx;}
</style>

