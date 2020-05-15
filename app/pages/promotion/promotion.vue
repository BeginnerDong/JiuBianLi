<template>
	<view>
		
		<view class="pdlr4 pdt15">
			<view class="proList proList-row">
				<view class="fs13 pdb10">促销活动说明：{{mainData.title}} {{mainData.combine_price}}/{{mainData.combine_count}}瓶</view>
				<view class="item flexRowBetween">
					<view class="pic">
						<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{mainData.title}}</view>
						
						<view class="flexRowBetween B-price">
							<view class="flex pubColor">
								<view class="fs11">促销价：</view>
								<view class="price fs14">{{mainData.combine_price}}</view>
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
						<image :src="mainData.mainImg&&mainData.mainImg[0]?mainData.mainImg[0].url:''" mode=""></image>
					</view>
					<view class="infor">
						<view class="title avoidOverflow">{{mainData.title}}</view>
						
						<view class="flexRowBetween B-price">
							<view class="flex pubColor">
								<view class="fs11">促销价：</view>
								<view class="price fs14">{{mainData.price}}</view>
							</view>
							<view class="flexEnd">×{{mainData.count}}</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="fxB-btn whiteBj">
			<view class="flexRowBetween fs12">
				<view class="item flexCenter">数量：<text class="red">{{count}}组</text></view>
				<view class="item flexCenter">总价：<text class="red">￥{{totalPrice}}</text></view>
				<!-- <view class="item flexCenter">节省：<text class="red">￥138.00</text></view> -->
			</view>
			<view class="addBtn flexCenter pubBj white" @click="addCar">加入购物车</view>
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
				count:1,
				totalPrice:0,
				mainData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.id = options.id;
			self.$Utils.loadAll(['getMainData'], self);
		},
		
		methods: {
			
			
			getMainData() {
				const self = this;
				const postData = {};
				postData.searchItem = {
					thirdapp_id: 2,
					id: self.id
				};
				postData.getAfter = {
					combineProduct:{
						tableName:'Product',
						middleKey:'combine_no',
						key:'product_no',
						searchItem:{
							status:1,
						},
						condition:'='
					},
				};
				 const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData = res.info.data[0];
						self.mainData.isSelect = true;
						self.mainData.behavior = 1;
						self.mainData.count = self.mainData.combine_count;
						self.totalPrice = self.mainData.combine_price
						const regex = new RegExp('<img', 'gi');
						self.mainData.content = self.mainData.content.replace(regex, `<img style="max-width: 100%;"`);
					} 
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.productGet(postData, callback);
			},
			
			counter(type) {
				const self = this;			
				if (type == '+') {
					self.count++;
					self.mainData.count += self.mainData.combine_count
				} else {
					if (self.count > 1) {
						self.count--;
						self.mainData.count -= self.mainData.combine_count
					}
				};			
				self.countTotalPrice();
			},
			
			countTotalPrice(){
				const self = this;
				self.totalPrice = self.count*self.mainData.combine_price;
				self.totalPrice = parseFloat(self.totalPrice).toFixed(2)
			},
			
			addCar(){
				const self = this;
				console.log(self.mainData)
				self.$Utils.setStorageArray('cartData',self.mainData,['id','behavior'],999);
				uni.showModal({
					title:'提示',
					content:'加入购物车成功',
					confirmText:'去购物车',
					cancelText:'继续购物',
					success(res) {
						if(res.confirm){
							self.Router.reLaunch({route:{path:'/pages/car/car'}})
						}else{
							uni.navigateBack({
								delta:1
							})
						}
					}
				})
			},
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

