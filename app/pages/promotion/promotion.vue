<template>
	<view>
		
		<view class="pdlr4 pdt15">
			<view class="proList proList-row">
				<view class="fs13 pdb10">促销活动说明：50°汾阳王青花10 500ml 98/2瓶 买一送一</view>
				<view class="item flexRowBetween">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						
						<view class="flexRowBetween B-price">
							<view class="flex pubColor">
								<view class="fs11">促销价：</view>
								<view class="price fs14">56.00</view>
							</view>
							<view class="numBox flex">
								<view @click="counter('-')">-</view>
								<view class="num pubColor">{{count}}</view>
								<view @click="counter('+')">+</view>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="pdlr4 pdt5 pd15">
			<view class="proList proList-row">
				<view class="fs13 pdb10">本促销包含以下商品</view>
				<view class="item flexRowBetween">
					<view class="pic">
						<image src="../../static/images/home-img10.png" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">50°汾阳王青花10 500ml</view>
						
						<view class="flexRowBetween B-price">
							<view class="flex pubColor">
								<view class="fs11">促销价：</view>
								<view class="price fs14">56.00</view>
							</view>
							<view class="flexEnd">×2</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="fxB-btn whiteBj">
			<view class="flexRowBetween fs12">
				<view class="item flexCenter">数量：<text class="red">1组</text></view>
				<view class="item flexCenter">总价：<text class="red">￥98.00</text></view>
				<view class="item flexCenter">节省：<text class="red">￥138.00</text></view>
			</view>
			<view class="addBtn flexCenter pubBj white">加入购物车</view>
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
				count:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
				} else {
					if (self.count > 1) {
						self.count--;
					}
				};			
				self.countTotalPrice();
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
	@import "../../assets/style/proList.css";
	page{padding-bottom:230rpx;background: #F5F5F5;}
	.proList-row .item .pic{width: 240rpx;height: 240rpx;}
	.proList-row .item .infor{width: 65%;padding: 15px 15px 15px 0;}
	
	.fxB-btn{position: fixed; bottom: 0;left: 0;width: 100%;box-sizing: border-box;}
	.fxB-btn .item{width: 33.3%; line-height: 80rpx;}
	.fxB-btn .addBtn{width: 100%;line-height: 100rpx;}
</style>

