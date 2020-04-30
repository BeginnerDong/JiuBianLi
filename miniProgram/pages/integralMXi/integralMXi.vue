<template>
	<view>
		<view class="myExtendTop white">
			<view class="bigNum">{{userInfoData.score}}</view>
			<view class="yuan">总积分</view>
		</view>
		
		<view class="myRowBetween pdlr4 fs14" >
			<view class="item flexRowBetween pdtb15" v-for="(item,index) in mainData" :key="index">
				<view class="ll">
					<view class="avoidOverflow">{{item.count>0?'会员消费奖励':'积分兑换商品'}}</view>
					<view class="color9 mgt5">{{item.create_time}}</view>
				</view>
				<view class="rr red">{{item.count}}</view>
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
				score:'',
				wx_info:{},
				rewardData:[
					{},{},{}
				],
				mainData:[],
				userInfoData:{},
				searchItem:{
					thirdapp_id:2,
					type:3,
					count:['not in','0.00']
				},
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				}
			}
		},
		
		onLoad(options) {
			const self = this;
			//self.paginate = self.$Utils.cloneForm(self.$AssetsConfig.paginate);
			self.$Utils.loadAll(['getUserInfoData','getMainData'], self);
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
					} else {
						self.$Utils.showToast('没有更多了', 'none');
					};
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
				self.$apis.flowLogGet(postData, callback);
			},

		},
	};
</script>

<style>
	@import "../../assets/style/editInfor.css";
	
	/* 我的积分 */
	.myExtendTop{width: 100%; text-align: center;box-sizing: border-box;padding: 80rpx 4% 80rpx 4%; background: #ff2121;}
	.myExtendTop .money{font-size: 80rpx; padding: 0rpx 4%; line-height:88rpx;}
	.myExtendTop .money:before{content: "￥"; font-size: 50rpx;}
	.myExtendTop .bigNum{font-size: 80rpx; padding: 0rpx 4%; line-height:88rpx;}
	.myExtendTop .yuan{ font-size:28rpx; padding:10rpx 0 30rpx 0;}
	.myExtendTop .txBtn{ width: 200rpx; height: 60rpx;line-height: 60rpx; background: #333; color: #fff; border-radius: 6rpx; margin: 0 auto;font-size: 26rpx;}
	
	.myRowBetween .item{padding: 30rpx 0;border-bottom: 1px solid #eee;}
	
</style>
