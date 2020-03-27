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
						<view class="fs12 red">余额：{{userInfoData.balance}}</view>
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
				<view class="fs16 price mgr5">{{price}}</view>
				<!-- <view class="fs10 color9">已优惠￥110.00</view> -->
			</view>
			<view class="payBtn fs16 white" @click="goPay">确认下单</view>
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
				num:2,
				userInfoData:{},
				pay:{},
				price:0,
				ratioArray:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.orderId = options.id;
			self.ratioArray = [{ratio:uni.getStorageSync('user_info').thirdApp.custom_rule.new_ratio/100},
			{ratio:uni.getStorageSync('user_info').thirdApp.custom_rule.ordinary_ratio/100},{ratio:uni.getStorageSync('user_info').thirdApp.custom_rule.silver_ratio/100},
			{ratio:uni.getStorageSync('user_info').thirdApp.custom_rule.gold_ratio/100},{ratio:uni.getStorageSync('user_info').thirdApp.custom_rule.diamond_ratio/100},]
			self.$Utils.loadAll(['getUserInfoData'], self);
			
		},
		
		onShow() {
			const self = this;
			self.pay = uni.getStorageSync('payData');
			self.price = self.pay.wxPay.price;
			
		},
		
		methods: {
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					parent:{
						tableName:'Distribution',
						middleKey:'user_no',
						key:'child_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
						for (var i = 0; i < self.ratioArray.length; i++) {
							if(self.userInfoData.level==i){
								self.scoreRatio = self.ratioArray[i].ratio
							}
						}
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			changeNum(num){
				const self = this;
				if(num!=self.num){
					if(num==1){
						if(parseFloat(self.pay.wxPay.price)<parseFloat(self.userInfoData.balance)){
							self.pay.balance = {
								price:self.pay.wxPay.price
							};
							delete self.pay.wxPay
						}else{
							self.$Utils.showToast('余额不足', 'none')
						}
					}else if(num==2){
						self.pay.wxPay = {
							price:self.pay.balance.price
						};
						delete self.pay.balance
					}
					self.num = num
				}
			},
			
			goPay() {
				const self = this;	
				var rewardRatio = uni.getStorageSync('user_info').thirdApp.custom_rule.reward/100;
				const postData = self.$Utils.cloneForm(self.pay)	
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
				};	
				postData.payAfter = [];
				if(self.scoreRatio&&parseFloat(self.price)>0&&self.scoreRatio>0){
					postData.payAfter.push(
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type:3,
								count:parseFloat(self.scoreRatio*parseFloat(self.price)).toFixed(2),
								trade_info:'消费赠积分',
								account:1,
								thirdapp_id:2,
								user_no:uni.getStorageSync('user_info').user_no
							},
						},
					)
				};
				if(self.userInfoData.parent&&self.userInfoData.parent[0]&&parseFloat(self.price)>0&&rewardRatio>0){
					postData.payAfter.push(
						{
							tableName: 'FlowLog',
							FuncName: 'add',
							data: {
								type:4,
								count:parseFloat(self.scoreRatio*parseFloat(self.price)).toFixed(2),
								trade_info:'下级消费返红包',
								account:1,
								thirdapp_id:2,
								user_no:self.userInfoData.parent[0].parent_no
							},
						},
					)
				};
				const callback = (res) => {
					if (res.solely_code == 100000) {
						uni.setStorageSync('canClick', true);
						if (res.info) {
							const payCallback = (payData) => {
								console.log('payData', payData)
								if (payData == 1) {
									uni.showToast({
										title: '支付成功',
										duration: 1000,
										success: function() {
											
										}
									});
									setTimeout(function() {
										self.$Router.redirectTo({route:{path:'/pages/user_order/user_order'}})
									}, 1000);
								} else {
									uni.setStorageSync('canClick', true);
									uni.showToast({
										title: '支付失败',
										duration: 2000
									});
								};
							};
							self.$Utils.realPay(res.info, payCallback);
						} else {
							
							uni.showToast({
								title: '支付成功',
								duration: 1000,
								success: function() {
									
								}
							});
							setTimeout(function() {
								self.$Router.redirectTo({route:{path:'/pages/user_order/user_order'}})
							}, 1000);
						};
					} else {
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};
				};
				self.$apis.pay(postData, callback);
			},
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

