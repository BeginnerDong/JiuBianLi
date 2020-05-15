<template>
	<view>
		<view class="orderNav flex fs14 whiteBj mgb10 color6">
			<view class="tt" :class="current==1?'on':''" @click="change('1')">全部</view>
			<view class="tt" :class="current==2?'on':''" @click="change('2')">待确认</view>
			<view class="tt" :class="current==3?'on':''" @click="change('3')">配送中</view>
			<view class="tt" :class="current==4?'on':''" @click="change('4')">已完成</view>
			<view class="tt" :class="current==5?'on':''" @click="change('5')">已评价</view>
		</view>
		<view class="pdlr4">
			<view class="proList proList-row">
				<view class="item" v-for="(item,index) in mainData" :key="index">
					<view class="flexRowBetween mglr4 pdtb10">
						<view class="fs12 color9">交易时间：{{item.create_time}}</view>
						<view class="fs12 red" v-if="item.pay_status==0">待确认</view>
						<view class="fs12 red" v-if="item.pay_status==1&&(item.transport_status==1||item.transport_status==0)">配送中</view>
						<view class="fs12 red" v-if="item.pay_status==1&&item.transport_status==2">已完成</view>
						<view class="fs12 red" v-if="item.isremark==1">已评价</view>
					</view>
					<view class="flexRowBetween" v-if="item.child" v-for="(c_item,c_index) in item.child">
						<view class="pic">
							<image :src="c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product
							&&c_item.orderItem[0].snap_product.mainImg&&c_item.orderItem[0].snap_product.mainImg[0]?c_item.orderItem[0].snap_product.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor" v-if="c_item.behavior==1">
							<view class="title avoidOverflow">
							{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}
							{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.combine_price:''}}元/{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.combine_count:''}}瓶
							</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.combine_price:''}}</view>
								<view class="flex">×{{c_item.count/c_item.orderItem[0].snap_product.combine_count}}</view>
							</view>
						</view>
						<view class="infor" v-else>
							<view class="title avoidOverflow">
							{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.title:''}}
							</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{c_item.orderItem&&c_item.orderItem[0]&&c_item.orderItem[0].snap_product?c_item.orderItem[0].snap_product.price:''}}</view>
								<view class="flex">×{{c_item.count}}</view>
							</view>
						</view>
					</view>
					<!-- <view class="mglr4 pdtb15" v-if="item.isremark==1">
						<view class="pdlr4 pdt10 pdb10 f5bj radius8 fs12">科就回归考虑到双方各烘干机撒联合国何健飞道快乐十分个分类山沟加工费是发的是</view>
					</view> -->
					<view class="pdlr4">
						<view class="flexEnd pdtb15">共{{item.totalCount}}件商品 合计:￥{{item.price}}</view>
						<div class="underBtn flexEnd pdb15">
							<span class="Bbtn" v-if="item.pay_status==0" @click="goPay(index)">去支付</span>
							<view class="Bbtn" v-if="item.pay_status==1&&item.transport_status==2&&item.isremark==0" :data-id="item.id"
							@click="Router.navigateTo({route:{path:'/pages/user_orderPingJia/user_orderPingJia?id='+$event.currentTarget.dataset.id}})">去评价</view>
						</div>
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
				current:1,
				searchItem:{
					level:1
				},
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getMainData'], self);
		
		},
		
		
		onReachBottom() {
			console.log('onReachBottom')
			const self = this;
			if (!self.isLoadAll && uni.getStorageSync('loadAllArray')) {
				self.paginate.currentPage++;
				self.getMainData()
			};
		},
		
		methods: {
			
			change(current) {
				const self = this;
				if(current!=self.current){
					self.current = current;
					if(self.current==1){
						delete self.searchItem.pay_status;
						delete self.searchItem.transport_status;
						delete self.searchItem.isremark;
					}else if(self.current==2){
						self.searchItem.pay_status=0
						self.searchItem.transport_status=0
						delete self.searchItem.isremark
					}else if(self.current==3){
						self.searchItem.pay_status=1
						self.searchItem.transport_status=['in',[0,1]]
						delete self.searchItem.isremark
					}else if(self.current==4){
						self.searchItem.pay_status=1
						self.searchItem.transport_status=2
						delete self.searchItem.isremark
					}else if(self.current==5){
						self.searchItem.pay_status=1
						self.searchItem.isremark=1
					}
					self.getMainData(true)
				}
			},
			
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
				postData.tokenFuncName = 'getProjectToken';
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'product_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					sku:{
						tableName:'Sku',
						middleKey:'sku_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					},
					payLog:{
						tableName:'PayLog',
						middleKey:'pay_no',
						key:'pay_no',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].totalCount = 0;
							if(self.mainData[i].child){
								for (var j = 0; j < self.mainData[i].child.length; j++) {
									self.mainData[i].totalCount += self.mainData[i].child[j].count
								}
							}
							
						}
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
			
			
			
			goPay(index){
				const self = this;
				console.log
				self.orderId = self.mainData[index].id;
				
				self.pay={
					wxAppPay:{
						price:parseFloat(self.mainData[index].price)
					}
				};
				self.orderId = self.mainData[index].id;
				uni.setStorageSync('payData',self.pay)
				self.Router.redirectTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay?id='+self.orderId}})
				
				/* if(self.mainData[index].payLog[0]){
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
								self.getMainData(true)
							}, 1000);
						} else {
							uni.setStorageSync('canClick', true);
							uni.showToast({
								title: '支付失败',
								duration: 2000
							});
						};
					};
					self.$Utils.realPay(self.mainData[index].payLog[0].wx_prepay_info, payCallback);
				}else{
					self.pay={
						wxPay:{
							price:parseFloat(self.mainData[index].price)
						}
					};
					self.orderId = self.mainData[index].id;
					uni.setStorageSync('payData',self.pay)
					self.Router.redirectTo({route:{path:'/pages/orderConfirm-pay/orderConfirm-pay?id='+self.orderId}})
				} */
			},
		}
	};
</script>

<style>
	@import "../../assets/style/NavTab.css";
	@import "../../assets/style/proList.css";
	page{padding-bottom: 110rpx;background: #F5F5F5;}
	.orderNav .tt{width: 20%;}
	.proList-row .item .pic{width: 160rpx;height: 160rpx;}
	.proList-row .item .infor{height: 160rpx;width: 75%;}
	
	.underBtn .Bbtn{display: block; padding: 0 20rpx; line-height: 50rpx; border:1px solid #FF2121; color: #FF2121;text-align: center; margin-left: 30rpx; border-radius: 8rpx; font-size: 26rpx;}
</style>

