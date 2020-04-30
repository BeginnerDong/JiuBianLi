<template>
	<view>
		
		<view class="pdlr4 whiteBj">
			<view class="pdtb15 flexRowBetween">
				<view class="">
					<view>营业时间</view>
					<view class="fs14 ftw">10:00~22:30</view>
				</view>
				<view class="seltSwitch fs12 flexCenter center">
					<view class="child" :class="submitData.time_type==1?'on':''" 
					@click="changeCurr('1')">立即送达</view>
					<view class="child" :class="submitData.time_type==2?'on':''" 
					@click="changeCurr('2')">预约配送</view>
				</view>
			</view>
		</view>
		
		<view class="mglr4 pdtb10">
			<view class="whiteBj radius10 mgb15">
				<view class="flex"><image class="w" style="" src="../../static/images/the-order-img.png" mode="widthFix"></image></view>
				<view class="pdtb10 mglr4 flexRowBetween fs13 borderB1" v-show="submitData.time_type==2">
					<view>选择时间</view>
					<!-- <view class="flexEnd color6 fs12">
						<view>请选择时间</view>
						
					</view> -->
					<hTimePicker sTime="10" cTime="23" interval="15" @changeTime="changeTime">
					  <view slot="pCon" class="changeTime">
					    {{submitData.book_time==''?'请选择时间':submitData.book_time}}
						
					  </view>
					</hTimePicker>
				</view>
				<view class="flexRowBetween pdtb15 mglr4"  
				@click="Router.navigateTo({route:{path:'/pages/user_address/user_address'}})">
					<view class="fs13" v-if="addressData.city">
						<view>{{addressData.city+addressData.detail}}</view>
						<view class="color6 flex mgt5">
							<view class="mgr10">{{addressData.name}}</view>
							<view class="">{{addressData.phone}}</view>
						</view>
					</view>
					<view class="fs13" v-else>
						请选择收货地址
					</view>
					<view class="flexEnd" style="width: 20%;">
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
			
			<view class="whiteBj radius10  mgb15">
				<view class="proList proList-row">
					<view class="item flexRowBetween"  v-for="(item,index) in mainData" :key="index">
						<view class="pic">
							<image :src="item.product&&item.product.mainImg&&item.product.mainImg[0]?item.product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">{{item.product?item.product.title:''}}</view>
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.product?item.product.price:''}}</view>
								<view class="flex">×{{item.count}}</view>
							</view>
						</view>
					</view>
				</view>
				<view class="myRowBetween pdlr4">
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw">红包</view>
						<view class="rr fs12">
							<view style="color:#ff2121" v-if="pay.bonus&&pay.bonus.price>0">红包抵扣{{pay.bonus.price}}元</view>
							<view class="color9" v-else>暂无红包可用</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
					</view>
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw">优惠券</view>
						<view class="rr fs12" v-if="couponData.length==0">
							<view class="color9 pubColor">暂无优惠券使用</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
						<view class="rr fs12" v-if="couponData.length>0&&chooseCoupon.length==0"
						@click="Router.navigateTo({route:{path:'/pages/order-useCoupon/order-useCoupon'}})">
							<view class="color9 pubColor" style="color:#ff2121">{{couponData.length}}张优惠券可用</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
						<view class="rr fs12" v-if="couponData.length>0&&chooseCoupon.length>0"
						@click="Router.navigateTo({route:{path:'/pages/order-useCoupon/order-useCoupon'}})">
							<view class="color9 pubColor" style="color:#ff2121">优惠券抵扣-{{pay.coupon[0].price}}</view>
							<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
						</view>
						
					</view>
					<view class="item flexRowBetween">
						<view class="ll fs13 ftw"></view>
						<view class="rr fs12 color6">
							合计<view class="price ftw fs15 mgl5">{{pay.wxPay&&pay.wxPay.price?pay.wxPay.price:'0'}}</view>
						</view>
					</view>
				</view>
			</view>
			
			<view class="myRowBetween pdlr4 whiteBj radius10  mgb15">
				<view class="item flexRowBetween" style="border-top: 0;">
					<view class="ll fs13 ftw">温度</view>
					<view class="rr fs12 color9" @click="tmptShow">
						{{specsDate[submitData.temperature]}}
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll fs13 ftw">订单备注</view>
					<view class="rr fs12 color9" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-remarks/orderConfirm-remarks'}})">
						<view class="avoidOverflow">{{submitData.passage1==''?'请填写备注内容':submitData.passage1}}</view>
						<image class="arrowR"  src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
				<view class="item flexRowBetween">
					<view class="ll fs13 ftw">发票信息</view>
					<view class="rr fs12 color9" @click="Router.navigateTo({route:{path:'/pages/orderConfirm-invoice/orderConfirm-invoice'}})">
						<view>{{submitData.receipt==0?'不开发票':'需要发票'}}</view>
						<image class="arrowR" src="../../static/images/arrow-icon.png" mode=""></image>
					</view>
				</view>
			</view>
		</view>
		
		<!-- 底部菜单按钮 -->
		<view class="xqbotomBar flexRowBetween">
			<view class="left flex mgl5">
				<view class="fs16 price mgr5">{{pay.wxPay&&pay.wxPay.price?pay.wxPay.price:'0'}}</view>
				<view class="fs10 color9" v-if="couponTotalPrice>0">已优惠￥{{couponTotalPrice}}</view>
			</view>
			<view class="payBtn fs16 white" @click="addOrder()">确认下单</view>
		</view>
		<!-- 底部菜单按钮 end -->
		
		<!-- 请选择温度 -->
		<view class="showSel" v-show="is_tmptShow">
			<view class="showSelCont fix">
				<view class="flexRowBetween pdtb15 center borderB1">
					<view class="color6 fs12" @click="tmptShow">取消</view>
					<view>{{specsDate[submitData.temperature]}}</view>
					<view class="color6 fs12" @click="tmptShow">确定</view>
				</view>
				<view class="pdtb15" style="height:300rpx;">
					<view class="selt_specs color6 pdb10 flex fs13">
						<view class="item" :class="submitData.temperature==index?'on':''" 
						@click="changeSpecs(index)" v-for="(item,index) in specsDate" :key="index" >
							<view class="item-tit">{{item}}</view>
						</view>
					</view>
					<view class="fs13 color9">仅限啤酒和饮料选择温度</view>
				</view>
			</view>
		</view>
		 
	</view>
</template>

<script>
	import hTimePicker from "@/components/h-timePicker/h-timePicker.vue";
	export default {
		components: { hTimePicker },
		data() {
			return {
				is_show:false,
				curr:1,
				proListDate:[{},{}],
				is_tmptShow:false,
				specsNum:0,
				specsDate:['冰镇','常温','半冰半常温'],
				Router:this.$Router,
				Utils: this.$Utils,
				addressData:{},
				mainData:[],
				totalPrice:0,
				userInfoData:{},
				pay:{
					coupon:[]
				},
				submitData:{
					level:1,
					receipt:0,
					time_type:1,
					temperature:0,
					passage1:'',
					book_time:'',
					
				},
				couponData:[],
				chooseCoupon:[],
				couponTotalPrice:0,
				receiptData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			uni.removeStorageSync('couponId');
			uni.removeStorageSync('passage1');
			uni.removeStorageSync('receiptData');
			self.mainData = self.$Utils.getStorageArray('orderList')[0];
			self.submitData.city_id = uni.getStorageSync('city_id');
			console.log(self.mainData)
			self.$Utils.loadAll(['getUserInfoData','getUserCouponData'], self);
		},
		
		onShow() {
			const self = this;
			if(uni.getStorageSync('choosedAddressData')){
				self.addressData = uni.getStorageSync('choosedAddressData')
			}else{
				self.getAddressData()
			};
			if(uni.getStorageSync('receiptData')){
				self.submitData.receipt = 1;
				self.receiptData = uni.getStorageSync('receiptData')
			}
			if(uni.getStorageSync('passage1')){
				self.submitData.passage1 = uni.getStorageSync('passage1');
			}
			console.log(uni.getStorageSync('couponId'))
			if(uni.getStorageSync('couponId')>0){
				console.log(432432)
				self.useCoupon(uni.getStorageSync('couponId'))
			}
			
		},
		
		
		methods: {
			
			useCoupon(id) {
				const self = this;	
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
			/* 	console.log(index)
				console.log(self.couponData);
				console.log(self.couponData[0]); */
				var id = id;
				
				var findCoupon = self.$Utils.findItemInArray(self.couponData, 'id', id);
				var findItem = self.$Utils.findItemInArray(self.pay.coupon, 'id', id);
				console.log('findCoupon', findCoupon)
				self.showCoupon = false;
				if(self.pay.coupon.length>=1){
					self.pay.coupon = []
					self.chooseCoupon = []
				};
				if (findCoupon) {
					findCoupon = findCoupon[1];
					var findSameCoupon =  self.$Utils.findItemsInArray(self.pay.coupon, 'product_id', id);
				} else {
					self.$Utils.showToast('优惠券错误', 'none');
					return;
				};
				if (findItem) {
					self.pay.coupon.splice(findItem[0], 1);
					self.chooseCoupon = []
				} else {
					console.log('self.data.price - self.data.couponTotalPrice',self.totalPrice - self.couponTotalPrice);
					console.log('findCoupon.condition',findCoupon.condition);
					if ((self.totalPrice - self.couponTotalPrice) < findCoupon.condition) {
						self.$Utils.showToast('未达满减标准', 'none');				
						return;
					};			
					 console.log('findSameCoupon.length', findSameCoupon.length)
					if (self.pay.coupon.length >= 1) {
						self.$Utils.showToast('叠加使用超限', 'none');
					
						return;
					};
					if (findCoupon.type == 1) {
						var couponPrice = findCoupon.value;
						console.log('findCoupon.discount', findCoupon.discount)
					} else if (findCoupon.type == 2) {
						
						var couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(findCoupon.discount / 100 * self.totalPrice)
							.toFixed(2);
					};
					if (parseFloat(couponPrice) + parseFloat(self.couponTotalPrice) > parseFloat(self.totalPrice)) {
						couponPrice = parseFloat(self.totalPrice).toFixed(2) - parseFloat(self.couponTotalPrice).toFixed(2);
					};
					self.pay.coupon.push({
						id: id,
						price: couponPrice.toFixed(2),
					});
					self.chooseCoupon.push({
						id: id,
						price: couponPrice,
					});
				};
				self.countTotalPrice();
			},
			
			getUserCouponData() {
				const self = this;
				var now = Date.parse(new Date());
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					use_step: 1,
					type: ['in', [1, 2]],
					invalid_time:['>',now]
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.couponData.push.apply(self.couponData, res.info.data)
					}
					self.countTotalPrice();
					self.$Utils.finishFunc('getUserCouponData');
					
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
			changeTime(e){
				const self = this;
				self.submitData.book_time = e
				console.log(e)
			},
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken'
				postData.searchItem = {
					user_no: uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.solely_code == 100000 && res.info.data[0]) {
						self.userInfoData = res.info.data[0];
					} else {
						self.$Utils.showToast(res.msg, 'none')
					};
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getAddressData() {
				const self = this;		
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					isdefault:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.addressData = res.info.data[0];
					}
				};
				self.$apis.addressGet(postData, callback);
			},
			
			countTotalPrice() {
				const self = this;
				self.totalPrice = 0;
				self.bonusPrice = 0;
				var bonusLimit = parseFloat(uni.getStorageSync('user_info').thirdApp.custom_rule.bonus_limit);
				self.couponTotalPrice = self.$Utils.addItemInArray(self.pay.coupon, 'price');
				
				if(parseFloat(self.userInfoData.bonus)>0){
					if(parseFloat(self.userInfoData.bonus)<bonusLimit){
						self.bonusPrice = self.userInfoData.bonus;
					}else{
						self.bonusPrice = bonusLimit;
					};
				}
				
				for (var i = 0; i < self.mainData.length; i++) {
					self.totalPrice += self.mainData[i].product.price * self.mainData[i].count;
				};
				console.log('self.totalPrice',self.totalPrice)
				console.log('self.couponTotalPrice',self.couponTotalPrice)
				console.log('self.bonusPrice',parseFloat(self.bonusPrice))
				var wxPay = parseFloat(self.totalPrice) - parseFloat(self.couponTotalPrice)-parseFloat(self.bonusPrice);
				console.log('wxPay',wxPay)
				if (wxPay > 0) {
					self.pay.wxPay = {
						price: wxPay.toFixed(2),
					};
				} else {
					  delete self.pay.wxPay;	 
				};
				if(parseFloat(self.bonusPrice)>0){
					self.pay.bonus = {
						price:parseFloat(self.bonusPrice).toFixed(2)
					}
				}
				console.log(self.pay)
			},
			
			addOrder() {
				const self = this;	
				/* if(self.orderId){
					uni.setStorageSync('payData',self.pay)
					self.Router.navigateTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay?id='+self.orderId}})
				}; */
				if(JSON.stringify(self.addressData)=='{}'){
					self.$Utils.showToast('收货地址不能为空', 'none');
					return
				};
				const postData = {}; 
				postData.orderList = self.$Utils.cloneForm(self.mainData);
				postData.data = self.$Utils.cloneForm(self.submitData);
				postData.parent = 1;
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.orderId = res.info.id;
						uni.setStorageSync('payData',self.pay)
						self.Router.redirectTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay?id='+self.orderId}})
						for (var i = 0; i < self.mainData.length; i++) {
							self.$Utils.delStorageArray('cartData', self.mainData[i], 'id');
						}
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
			
			goPay(order_id) {
				const self = this;	
				
				const postData = self.$Utils.cloneForm(self.pay)	
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
										self.$Router.redirectTo({route:{path:'/pages/user_myOrder/user_myOrder'}})
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
								self.$Router.redirectTo({route:{path:'/pages/user_myOrder/user_myOrder'}})
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
			
			changeCurr(curr){
				const self = this;
				if(curr!=self.submitData.time_type){
					self.submitData.time_type=curr
				}
			},
			
			tmptShow(){
				const self = this;
				self.is_tmptShow = !self.is_tmptShow
			},
			
			changeSpecs(index){
				const self = this;
				if(self.submitData.temperature!=index){
					self.submitData.temperature=index
				}
			},
			
			
		}
	};
</script>

<style>
	@import "../../assets/style/proList.css";
	@import "../../assets/style/editInfor.css";
	@import "../../assets/style/proDetail.css";
	page{background: #F5F5F5;padding-bottom: 110rpx;}
	.seltSwitch{width: 320rpx;height: 60rpx;line-height: 60rpx;background: #ededed;border-radius: 30rpx;}
	.seltSwitch .child{width: 50%;}
	.seltSwitch .child.on{background: #FF2121;color: #fff;border-radius: 30rpx;}

	.proList-row .item{margin-bottom: 0;}
	.proList-row .item .pic{width: 160rpx; height: 160rpx;padding: 20rpx;}
	.proList-row .item .infor{height: 200rpx; width: 70%;}
	
	.myRowBetween .item{padding: 30rpx 0;border-top: 1px solid #eee;}
	
	/* 温度弹框 */
	.showSel{width: 100%;height: 100%;background: rgba(0, 0, 0, 0.5);position: fixed;top: 0;right: 0;bottom: 0;left: 0;z-index: 10000;}
	.showSelCont {padding:0 4%;background: #fff;position: fixed;bottom: 0;left: 0;box-sizing: border-box;width: 100%;border-radius: 20rpx 20rpx 0 0;}
	.selt_specs{flex-wrap: wrap;}
	.selt_specs .item{padding: 10rpx 30rpx;border-radius:10rpx;background: #F5F5F5;margin-right: 28rpx;box-sizing: border-box; margin-bottom: 20rpx;}
	.selt_specs .item.on{color: #fff;background: #FF2121;}
</style>

