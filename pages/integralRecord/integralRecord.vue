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
				<view class="item" v-for="(item,index) in mainData">
					<view class="flexRowBetween mglr4 pdtb10">
						<view class="fs12 color9">交易时间：{{item.create_time}}</view>
						<view class="fs12 red" v-if="item.pay_status==0">待确认</view>
						<view class="fs12 red" v-if="item.pay_status==1&&(item.transport_status==1||item.transport_status==0)">配送中</view>
						<view class="fs12 red" v-if="item.pay_status==1&&item.transport_status==2">已完成</view>
			
					</view>
					<view class="flexRowBetween">
						<view class="pic">
							<image :src="item.mainImg&&item.mainImg[0]?item.mainImg[0].url:''" mode=""></image>
						</view>
						<view class="infor">
							<view class="title avoidOverflow">{{item.title}}</view>
							
							<view class="flexRowBetween B-price">
								<view class="price fs14">{{item.price}}</view>
								<view class="flex">×{{item.count}}</view>
							</view>
						</view>
					</view>
					<view class="pdlr4">
						<view class="flexEnd pdtb15">共{{item.count}}件商品 合计:￥{{item.price}}</view>
						<div class="underBtn flexEnd pdb15" v-if="item.pay_status==0">
							<span class="Bbtn">确认兑换</span>
						</div>
						<view class="underBtn flexEnd pdb15" v-if="item.pay_status==1&&item.transport_status==1">
							<view class="Bbtn">确认收货</view>
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
				current:1,
				searchItem:{
					type:2
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
					}else if(self.current==3){
						self.searchItem.pay_status=1
						self.searchItem.transport_status=1
					}else if(self.current==4){
						self.searchItem.pay_status=1
						self.searchItem.transport_status=2
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
				
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.orderGet(postData, callback);
			},
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

