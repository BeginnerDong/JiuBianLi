<template>
	<view>
		<view class="pdlr4">
			<view class="myRowBetween">
				<view class="pdt15 ftw">推荐支付方式</view>
				<view class="item pdtb15 borderB1 flexRowBetween" @click="changeNum('1')">
					<view class="ll flex">
						<view class="payIcon">
							<image src="../../static/images/pay-icon.png" mode=""></image>
						</view>
						<view>
							<view class="fs13 ftw">余额支付</view>
							<view class="fs12 color6">使用余额支付</view>
						</view>
					</view>
					<view class="rr" style="justify-content:space-between;">
						<view class="fs12 red">余额：0.00</view>
						<view>
							<image class="seltIcon" :src="num==1?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'" mode=""></image>
						</view>
					</view>
				</view>
				<view class="pdt15 ftw">其他支付方式</view>
				<view class="item pdtb15 borderB1 flexRowBetween"  @click="changeNum('2')">
					<view class="ll flex">
						<view class="payIcon">
							<image src="../../static/images/pay-icon1.png" mode=""></image>
						</view>
						<view class="fs13 ftw">微信支付</view>
					</view>
					<view class="rr">
						<view>
							<image class="seltIcon" :src="num==2?'../../static/images/shopping-icon3.png':'../../static/images/shopping-icon2.png'" mode=""></image>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left flex mgl5">
				<view class="fs16 price mgr5">856.00</view>
				<view class="fs10 color9">已优惠￥110.00</view>
			</view>
			<view class="payBtn fs16 white" @click="Router.redirectTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay'}})">确认下单</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
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
				num:1
			}
		},
		
		onLoad(options) {
			const self = this;
			// self.$Utils.loadAll(['getMainData'], self);
		},
		methods: {
			changeNum(num){
				const self = this;
				if(num!=self.num){
					self.num = num
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
	@import "../../assets/style/proDetail.css";
	@import "../../assets/style/editInfor.css";
	page{padding-bottom: 130rpx;}
	
	.payIcon{ width: 60rpx; height: 60rpx;margin-right: 20rpx;}
	.payIcon image{width: 100%;height: 100%;border-radius: 50%;}
	.seltIcon{width: 36rpx;height: 36rpx;}
	
</style>

