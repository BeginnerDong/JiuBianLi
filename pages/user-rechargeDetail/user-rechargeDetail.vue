<template>
	<view>
		
		<view class="rechgeMx">
			<view class="item flexRowBetween" v-for="item in mainData">
				<view class="tt">
					<view>充值</view>
					<view class="fs12 color6">{{item.create_time}}</view>
				</view>
				<view class="tt price flexCenter">{{item.price}}</view>
				<view class="tt flexEnd" v-if="item.receipt_status==0"><view class="btn" :data-id="item.id"
				@click="Router.navigateTo({route:{path:'/pages/orderConfirm-invoice/orderConfirm-invoice?id='+$event.currentTarget.dataset.id}})">
				开电子发票</view></view>
				<view class="tt flexEnd" v-else><view class="btn finish">发票已开</view></view>
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
					type:6,
					pay_status:1,
					
				},
				userInfoData:{},
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					pagesize: 10,
					is_page: true,
				},
				current:1
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.paginate);
			//self.$Utils.loadAll(['getMainData'], self)
		
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
				self.$apis.orderGet(postData, callback);
			},

		},
	};
</script>

<style>
@import "../../assets/style/xieyiAlert.css";

.rechgeMx .item{padding: 20rpx 4%;border-bottom: 1px solid #eee;}
.rechgeMx .item .tt{width: 33.3%;}
.rechgeMx .item .btn{width: 150rpx;text-align: center; line-height: 50rpx;padding: 0 10rpx;border-radius: 8rpx; color: #fff;background: #FF2121; font-size: 24rpx;box-sizing: border-box;}
.rechgeMx .item .btn.finish{background: #ddd;}
</style>

