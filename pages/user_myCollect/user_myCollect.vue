<template>
	<view>
		<view class="proList flex pdlr4 mgt15">
			<view class="item" v-for="(item,index) in mainData" :key="index" :data-id="item.product[0].id"
			@click="Router.navigateTo({route:{path:'/pages/prodetail/prodetail?id='+$event.currentTarget.dataset.id}})">
				<view class="pic">
					<image :src="item.product&&item.product[0]&&item.product[0].mainImg
					&&item.product[0].mainImg[0]?item.product[0].mainImg[0].url:''" mode=""></image>
				</view>
				<view class="infor">
					<view class="title avoidOverflow">{{item.product&&item.product[0]?item.product[0].title:''}}</view>
					<view class="flexRowBetween">
						<view class="price">{{item.product&&item.product[0]?item.product[0].price:''}}</view>
						<view class="yuanJia">{{item.product&&item.product[0]?item.product[0].o_price:''}}</view>
					</view>
				</view>
			</view>
		</view>
		<view class="R-Fixbtn" @click="Router.redirectTo({route:{path:'/pages/car/car'}})">
			<view class="num">{{cartData.length}}</view>
			<view class="icon oh">
				<image src="../../static/images/details-icon9.png" mode=""></image>
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
				is_show:false,
				wx_info: {},
				proList:[{},{},{}],
				mainData:[],
				paginate:{
					count: 0,
					currentPage: 1,
					is_page: true,
					pagesize: 10
				},
				cartData:[]
			}
		},
		
		onLoad(options) {
			const self = this;
			self.paginate = self.$Utils.cloneForm(self.paginate);
			self.cartData = self.$Utils.getStorageArray('cartData');
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
					type:2,
					user_no:uni.getStorageSync('user_info').user_no
				};
				postData.getAfter = {
					product:{
						tableName:'Product',
						middleKey:'relation_id',
						key:'id',
						searchItem:{
							status:1
						},
						condition:'='
					}
				};
				const callback = (res) => {
					if (res.info.data.length > 0) {
						self.mainData.push.apply(self.mainData, res.info.data);
					}
					console.log('self.mainData', self.mainData)
					self.$Utils.finishFunc('getMainData');
				};
				self.$apis.logGet(postData, callback);
			},
		}
	}
</script>

<style>
	@import "../../assets/style/proList.css";
	page{ background: #f5f5f5;}
	.R-Fixbtn .num{line-height: 30rpx; min-width:30rpx;border-radius: 20rpx;background: #FF2121;color: #fff;font-size: 20rpx;position: absolute; top: 0;right: 0rpx;box-sizing: border-box;padding: 0 6rpx;}
	.R-Fixbtn .icon{padding: 26rpx; box-sizing: border-box;background: #fff;border-radius: 50%;box-shadow: 0 2rpx 10rpx rgba(0,0,0,0.15);}
</style>
