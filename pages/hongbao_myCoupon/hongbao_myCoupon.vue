<template>
	<view>
		
		<view class="couponlist list mglr4 pdt15">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="flex ll">
					<view class="mny">{{item.value}}</view>
					<view class="infor fs12">
						<view>{{item.value}}元优惠券</view>
						<view class="flex mgt15"><view class="labBtn">满{{item.condition}}元使用</view></view>
					</view>
				</view>
				<!-- <view class="rr flexCenter">
					<view class="lq-btn">领取</view>
				</view> -->
			</view>
		</view>
		
		<view class="submitbtn" style="margin-top: 100rpx;">
			<button class="btn" type="button"  @click="Router.back({route:{path:'/pages/hongbao_Coupon/hongbao_Coupon'}})">领取更多优惠</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Router:this.$Router,
		
				couponShowData:[
					{price:'5',title:'5元优惠券',lable:'香烟不可用',infor:'满50元使用',},
					{price:'10',title:'10元优惠券',lable:'香烟不可用',infor:'满100元使用'}
				],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				},
				mainData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.paginate);
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
				postData.searchItem = {
					thirdapp_id: 2,
					use_step:1
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.userCouponGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/couponlist.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
</style>

