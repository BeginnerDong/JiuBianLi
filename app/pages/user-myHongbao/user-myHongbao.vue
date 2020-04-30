<template>
	<view v-if="mainData.length>0">
		<view class="pubBj center myHead white" style="height: 300rpx;">
			<view class="mny">{{userInfoData.bonus}}</view>
			<view class="fs12 mgt10">余额(元)</view>
		</view>
		 
		<view class="myRowBetween">
			<view class="item flexRowBetween" v-for="item in mainData" :key="index">
				<view class="ll">
					<view>{{item.count>0?'获得红包':'购买商品'}}</view>
					<view class="fs12 color9">{{item.create_time}}</view>
				</view>
				<view class="rr red">{{item.count}}</view>	
			</view>
		</view>
	</view>
	<view v-else>
		<view class="">
			<view class="nodata flexCenter">
				<image class="noIcon" src="../../static/images/the-wallet-icon.png" mode=""></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
				rewardData:[
					{},{},{}
				],
				searchItem:{
					type:4,
				},
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				},
				current:1,
				userInfoData:{}
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.paginate);
			self.$Utils.loadAll(['getUserInfoData'], self)
		},
			
		onShow() {
			const self =  this;
			self.getMainData(true);
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
			
			getUserInfoData() {
				const self = this;
				const postData = {};
				postData.tokenFuncName = 'getProjectToken';
				postData.searchItem = {
					user_no:uni.getStorageSync('user_info').user_no
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.userInfoData = res.info.data[0];
					}
					console.log('self.userInfoData', self.userInfoData)
					self.$Utils.finishFunc('getUserInfoData');
				};
				self.$apis.userInfoGet(postData, callback);
			},
			
			getMainData(isNew) {
				const self = this;
				if (isNew) {
					self.mainData = [];
					self.paginate = {
						count: 0,
						currentPage: 1,
						pagesize: 10,
						is_page: true,
					};
				};
				const postData = {};
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = self.$Utils.cloneForm(self.searchItem);
				postData.tokenFuncName = 'getProjectToken';
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
						for (var i = 0; i < self.mainData.length; i++) {
							self.mainData[i].create_time = self.mainData[i].create_time.substr(0,10)
						}
					}
					console.log(self.mainData)
				};
				self.$apis.flowLogGet(postData, callback);
			},

		},
	};
</script>

<style>
.myHead .mny{padding-top: 80rpx;font-size: 70rpx;line-height: 70rpx;}
.myRowBetween .item{padding: 20rpx 4%; border-bottom: 1px solid #eee;}
.noIcon{width: 298rpx;height: 328rpx; display: block; margin-top: 200rpx;}
</style>

