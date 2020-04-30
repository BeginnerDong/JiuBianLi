<template>
	<view>
		
		<view class="mglr4 pdtb15 flexEnd">
			<view class="myQuanBtn pubColor fs12 center"  
			@click="Router.navigateTo({route:{path:'/pages/hongbao_myCoupon/hongbao_myCoupon'}})">我的券</view>
		</view>
		<view class="couponlist list mglr4">
			<view class="item flexRowBetween" v-for="(item,index) in mainData" :key="index">
				<view class="flex ll">
					<view class="mny">{{item.value}}</view>
					<view class="infor fs12">
						<view>{{item.value}}元优惠券</view>
						<view class="flex mgt15"><view class="labBtn">满{{item.condition}}元使用</view></view>
					</view>
				</view>
				<view class="rr flexCenter">
					<view class="lq-btn" @click="submit(index)" v-if="item.hasPick&&item.hasPick.length<item.limit">领取</view>
					<view class="lq-btn" @click="submit(index)" style="color: #666;border: 1px solid #666;" 
					v-if="item.hasPick&&item.hasPick.length==item.limit">已领取</view>
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
				couponShowData:[
					{price:'5',title:'5元优惠券',lable:'香烟不可用',infor:'满50元使用',},
					{price:'10',title:'10元优惠券',lable:'香烟不可用',infor:'满100元使用'},
					{price:'15',title:'15元优惠券',lable:'香烟不可用',infor:'满200元使用'},
					{price:'20',title:'20元优惠券',lable:'香烟不可用',infor:'满300元使用'}
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
			
			submit(index){
				const self = this;
				self.orderList = [
					{coupon_id:self.mainData[index].id,count:1,type:self.mainData[index].type}
				];
				self.couponAdd()
			},
			
			couponAdd() {
				const self = this;
				var now =  (new Date()).getTime();
				const postData = {
					
					tokenFuncName: 'getProjectToken',
					
				};
				postData.couponList = self.$Utils.cloneForm(self.orderList);
				postData.pay = {
					score: 0
				};
				console.log('postData', postData)
				const callback = (res) => {
					if (res && res.solely_code == 100000) {
						self.$Utils.showToast('领取成功', 'none')
					} else {
						self.$Utils.showToast(res.msg, 'none')
					}
				};
				self.$apis.couponAdd(postData, callback);
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
				postData.paginate = self.$Utils.cloneForm(self.paginate);
				postData.searchItem = {
					thirdapp_id: 2,
				};
				postData.getAfter = {
					hasPick:{
						token:uni.getStorageSync('user_token'),
						tableName:'UserCoupon',
						middleKey:'coupon_no',
						key:'coupon_no',
						searchItem:{
							status:1,
						},
						condition:'='
					},
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.couponGet(postData, callback);
			},
			
		}
	};
</script>

<style>
	@import "../../assets/style/couponlist.css";
	page{padding-bottom: 60rpx;background: #F5F5F5;}
	
	.myQuanBtn{width: 112rpx; height: 40rpx;line-height: 40rpx;background: url(../../static/images/lingquan-img1.png) 0 0 /100% 100%;}
</style>

