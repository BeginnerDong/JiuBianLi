<template>
	<view>
		
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="flexRowBetween pdtb15 mglr4"  
				@click="Router.navigateTo({route:{path:'/pages/nearbyStore/nearbyStore'}})">
					<view class="fs13" v-if="addressData.address">
						<view>{{addressData.address}}</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">{{addressData.name}}</view>
						</view>
					</view>
					<view class="fs13" v-else>
						请选择提货门店
					</view>
					<view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10  mgb15">
				<view class="proList proList-row">
					<view class="item" >
						<view class="flexRowBetween" v-for="(item,index) in mainData.product" :key="index">
							<view class="pic">
								<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
							</view>
							<view class="infor">
								<view class="title avoidOverflow">{{item.product?item.product.title:''}}</view>
								<view class="flexRowBetween B-price">
									<view class="price fs14">{{item.product?item.product.price:''}}<text class="yuanJia">{{item.product?item.product.o_price:''}}</text></view>
									<view class="flex">×1</view>
								</view>
							</view>
						</view>
						<view class="flexEnd mglr4 pdt10 pdb15 fs12 color6">
							总计 <view class="red mgl10">积分：<text class="fs15">{{totalPrice}}</text></view>
						</view>
					</view>
				</view>
			</view>
			
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left flex mgl5 fs14 red">积分：<text class="fs16">{{totalPrice}}</text></view>
			<view class="payBtn fs16 white" @click="Utils.stopMultiClick(submit)">确认兑换</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				Utils: this.$Utils,
				addressData:{},
				mainData:[],
				totalPrice:0,
				userInfoData:{},
				specialProvince:[],
				totalScore:0
			}
		},

		onLoad(options) {
			const self = this;
			uni.setStorageSync('canClick',true);
			self.mainData = uni.getStorageSync('payPro');
			console.log('self.data.mainData', self.mainData);
	
			self.$Utils.loadAll(['getUserInfoData'], self);	
			
			
			self.countTotalPrice();
		},

		onShow() {
			const self = this;
			if(uni.getStorageSync('chooseShopData')){
				self.addressData = uni.getStorageSync('chooseShopData')
			}
		},

		methods: {
			
			
			formIdAdd(e) {
				const self = this;
				console.log(e)
				self.$apis.WxFormIdAdd(e.detail.formId, Date.parse(new Date()) / 1000 + 7 * 86400);
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};		
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData=res.info.data[0];	
					} else {
						self.$Utils.showToast(res.msg,'none');
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},	
			
			submit(){
				const self = this;
				uni.setStorageSync('canClick',false);
				/* if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('积分不足','none');
					return
				}
				self.addOrder() */
				if(JSON.stringify(self.addressData) == '{}'){
					uni.setStorageSync('canClick',true);
					self.$Utils.showToast('请选择提货门店','none')
				}else{
					if(parseFloat(self.userInfoData.score)<parseFloat(self.totalScore)){
						uni.setStorageSync('canClick',true);
						self.$Utils.showToast('积分不足','none');
						return
					}
					self.addOrder()
				}
			},
			
			addOrder() {
				const self = this;					
				const postData = self.$Utils.cloneForm(self.mainData)
				postData.tokenFuncName = 'getProjectToken';	
				postData.data = {
					shop_no:self.addressData.user_no
				};
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						self.pay()
					} else {		
						uni.setStorageSync('canClick', true);
						uni.showToast({
							title: res.msg,
							duration: 2000
						});
					};		
				};
				self.$apis.addOrder(postData, callback);
			},
			
			
			pay(order_id) {
				const self = this;	
				const postData = {};	
				postData.score = {
					price:self.totalPrice
				};
				postData.tokenFuncName = 'getProjectToken',
				postData.searchItem = {
					id: self.orderId
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
										self.$Router.redirectTo({route:{path:'/pages/integralShop/integralShop'}})
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
								self.$Router.redirectTo({route:{path:'/pages/integralShop/integralShop'}})
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
			
	
			
			
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;		
				self.totalScore = 0;
				for (var i = 0; i < self.mainData.product.length; i++) {
					self.totalPrice += self.mainData.product[i].product.price * self.mainData.product[i].count;
					self.totalScore += self.mainData.product[i].product.score * self.mainData.product[i].count;
				};
			},
			
			
		}
	}
</script>

<style>
	
	@import "../../assets/style/proList.css";
	@import "../../assets/style/proDetail.css";
	page{background: #F5F5F5;padding-bottom: 110rpx;}
	.proList-row .item{margin-bottom: 0;}
	.proList-row .item .pic{width: 160rpx; height: 160rpx;padding: 20rpx;}
	.proList-row .item .infor{height: 200rpx; width: 70%;}
</style>

